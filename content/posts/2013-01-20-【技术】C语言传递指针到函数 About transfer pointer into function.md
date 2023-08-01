---
layout: post
title: '【技术】C语言传递指针到函数 About transfer pointer into function'
date: 2013-01-20 15:11:00 +0800
category: from_cnblogs
slug: p20130120151100
---


<div style="line-height:21px; font-size:14px">
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">&nbsp; &nbsp;&nbsp;<strong>&nbsp;About transfer pointer into function (C Language)</strong></span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial"><br style="line-height:1.5">
</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">In the main() function of our program, we often need to use some user defined functions to do our calculating work.</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial"><br style="line-height:1.5">
</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">For a user_function, we may transfer into several int variable, and return a variable.</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">or, we may need modify a data array by calling a function. And return void.</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">or, we may need modify a data array in a struct, and call a function to modify that data array. We need to transfer into the function the pointer of that struct containing our data
 array.</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial"><br style="line-height:1.5">
</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">1. Transfer into function the pointer of An data array:</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial"><br style="line-height:1.5">
</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">&nbsp; &nbsp;Take the example of my project: &quot;Audio_Goertzel_Algorithm&quot;</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
【声数函，方括号；调函数，没括号】</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<pre name="code" class="cpp">        //--------Some delaration in main() function: ---------
    float data_Re[1024]; // Array of Real part of data
    float data_Im[1024]; // Array of image part of data 
    float abs_x[1024];
    int length_kArr = 8; // k is array of number of X
    int kArr[length_kArr];
    int num_data = 1024; // Length of array x[i]


    //--- Prepare the value of k[] ----
    kArr[0] = 87;   // kArr = [0:1:15] ; length_kArr = 16
    kArr[1] = 96;   // 770*num_data/fs
    ...
    kArr[7] = 204;

    //--------Process the data by calling the usr_function: goertzel() ----
    goertzel(data_Re, data_Im, num_data, kArr, length_kArr);  
    
    ///////////// The difinition of usr_function in the file &quot;user.c&quot;///////////
    void goertzel(float addr_Re[], float addr_Im[], int N, int kArr[], int length_kArr)
    {
        // Input the data for processing: // Here we can use &quot;static float array[...]&quot; as global var!
        float X_Re[1024];
        float X_Im[1024];

        memcpy( x_Re, addr_Re, 4*N);  //float occupy 4 bytes per element.
                // memcpy(void *dest, void *src, unsigned int count);
        memcpy( x_Im, addr_Im, 4*N);  //float occupy 4 bytes per element.
        // you can also process addr_Re instead of x_Re, 
        // so that you don't need to memcpy like above.

        //----Begin of processing ----------
        ...
        //---- End of data processing ---

        // Output the data processing result:
        memcpy(addr_Re, X_Re, 4*1024) ;  // address_Re store the adr of Re Array of process result
        memcpy(addr_Im, X_Re, 4*1024) ;  // address_Im store the adr of Im Array of process result
    
        return;
    }</pre><br>
<br>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
&nbsp; &nbsp;&nbsp;</div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">-------------End of section 1. ----------------</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial"><br style="line-height:1.5">
</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">Section 2. Transfer the pointer of Struct of a data array
<span style="font-size:14px; line-height:21px">into function</span>:</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial"><br style="line-height:1.5">
</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">&nbsp; &nbsp;Take the example of my project: &quot;Piano_demo&quot;</span></div>
<div style="line-height:21px; font-size:14px"><span style="font-family:Arial">&nbsp; &nbsp;in file &quot;common_data.h&quot;, the struct is defined here:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
【声结构，方括号；声调函，没括号】</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
&nbsp; &nbsp;<pre name="code" class="cpp">    struct data_1024res16 {
        int samplingrate;
        int datares;
        int nsamples;
        int nchannels;
        int16_t data[1024];  // Here is the data array that we're going to modify.

    };
        
    //--------Some delaration in main() function: ---------

    #include&quot;common_data.h&quot;
    struct data_1024res16 MyStructArr_y[1000]; // Declared 1000 StructArray, each StructArrady contains:
                    //int samplingrate;int datares;int nsamples;int nchannels;int16_t data[1024];
                    //int16_t data[1024] is 1024 16bit data buffer.

    //--------Process the data by calling the usr_function: sound_x() ----
    // modify the data in MyStructArr_y.data by calling this function
    sound_x(MyStructArr_y, fs, 494);  



    ///////////// The difinition of usr_function in the file &quot;user.c&quot;///////////
   
   

    void sound_x(struct data_1024res16* MyStructArr_y, int fs, int F)
    {

        short data1[1000];   // use 1000 &gt;&gt; 20 for sufficient. only 20 data1[] are used.
        //----Begin of processing ----------
        ...
        //---- End of data processing ---
    
        // Output the data processing result:
        for(i = 0; i &lt; 20; i++)
        {
            memcpy(MyStructArr_y[i].data, data1, 2*1000);
        }

        return;
    }
</pre><br>
</div>
</div>
   
