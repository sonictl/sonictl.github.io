---
layout: post
title:  "Medical Image Preprocessing with python"
date: 2023-09-19 16:02:27 +0800
categories: image_proc
slug: p20230919160227
---
# 1. Load data and get to know the meta data

```python
import pydicom
raw_medical_image = pydicom.read_file(file_path)
print(raw_medical_image)

medical_image = raw_medical_image.pixel_array
plt.imshow(medical_image, cmap='gray')      # cmap -- color map
```

# 2. Five steps of processing

## 2.1 Transforming to HU:

obtain the HU by using Rescale_Intercept and Rescale_Slope headers:

```
hu_image = image * medical_image.RescaleSlope + medical_image.RescaleIntercept

#window_image:
img_min = window_center - window_width // 2
img_max = window_center + window_width // 2
window_image = image.copy()
window_image[window_image < img_min] = img_min
window_image[window_image > img_max] = img_max
```

## 2.2 Removing Noises

```python
segmentation = morphology.dilation(brain_image, np.ones((1, 1)))  # 返回图像的灰度形态扩张, skimage
labels, label_nb = ndimage.label(segmentation)   # labels: num of connectivities
# label_nb: 2darray with element labeled by integers

label_count = np.bincount(labels.ravel().astype(np.int))
label_count[0] = 0

mask = labels == label_count.argmax()

mask = morphology.dilation(mask, np.ones((1, 1)))
mask = ndimage.morphology.binary_fill_holes(mask)
mask = morphology.dilation(mask, np.ones((3, 3)))
masked_image = mask * brain_image
```


## 2.3 Tilt Correction, 

Rotate the image, refer: 

https://towardsdatascience.com/medical-image-pre-processing-with-python-d07694852606


```python
img=numpy.uint8(iskemiMaskedImg)
contours, hier =cv2.findContours (img, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
mask=numpy.zeros(img.shape, numpy.uint8)
c = max(contours, key = cv2.contourArea)  # find the biggest contour (c) by the area

(x,y),(MA,ma),angle = cv2.fitEllipse(c)
rmajor = max(MA,ma)/2

cv2.ellipse(img, ((x,y), (MA,ma), angle), color=(0, 255, 0), thickness=2) # check fit result

if angle > 90:
angle -= 90
else:
angle += 90
xtop = x + math.cos(math.radians(angle))*rmajor
ytop = y + math.sin(math.radians(angle))*rmajor
xbot = x + math.cos(math.radians(angle+180))*rmajor
ybot = y + math.sin(math.radians(angle+180))*rmajor
cv2.line(img, (int(xtop),int(ytop)), (int(xbot),int(ybot)), (0, 255, 0), 3)

pylab.imshow(img)
pylab.show()

M = cv2.getRotationMatrix2D((x, y), angle-90, 1)  #transformation matrix

img = cv2.warpAffine(img, M, (img.shape[1], img.shape[0]), cv2.INTER_CUBIC)

pylab.imshow(img)
pylab.show()

```

## 2.4 Crop Images:

```python
    # create a mask with the background pixels
    mask = image == 0
    
    # Find the brain area
    coords = np.array(np.nonzero(~mask))
    top_left = np.min(coords, axis=1)
    bottom_right = np.max(coords, axis=1)
    
    # remove the background
    croped_image = image[top_left[0]:bottom_right[0], top_left[1]:bottom_right[1]]
```

## 2.5 Padding:

 紧接着Crop。找到图像上下左右沿并裁切出来图像区域以后，为图像扩充背景以满足原尺寸（e.g. 512*512）

 Cropping and Padding 的联合使用可使图像居中。

```python
def add_pad(image, new_height=512, new_width=512):
    height, width = image.shape

    final_image = np.zeros((new_height, new_width))
    
    pad_left = int((new_width - width) // 2)
    pad_top = int((new_height - height) // 2)
    
    # Replace the pixels with the image's pixels
    final_image[pad_top:pad_top + height, pad_left:pad_left + width] = image
    return final_image
    
def add_pad(image, new_height=512, new_width=512):
    height, width = image.shape
    final_image = np.zeros((new_height, new_width))

    pad_left = int((new_width - width) // 2)
    pad_top = int((new_height - height) // 2)

    # Replace the pixels with the image's pixels 
    final_image[pad_top:pad_top + height, pad_left:pad_left + width] = image
    
    return final_image
```



