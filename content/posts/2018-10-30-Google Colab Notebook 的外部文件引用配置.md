---
layout: post
title: 'Google Colab Notebook 的外部文件引用配置'
date: 2018-10-30 12:09:00 +0800
category: from_cnblogs
slug: p20181030120900
---
# Google Colab Notebook 的外部文件引用配置

Reference: [How to upload the file and read Google Colab](https://qiita.com/Rowing0914/items/51a770925653c7c528f9)

1. 先装工具：`google-drive-ocamlfuse`

    ```
    !apt-get install -y -qq software-properties-common python-software-properties module-init-tools
    !add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null
    !apt-get update -qq 2>&1 > /dev/null
    !apt-get -y install -qq google-drive-ocamlfuse fuse
    from google.colab import auth
    auth.authenticate_user()
    from oauth2client.client import GoogleCredentials
    creds = GoogleCredentials.get_application_default()
    import getpass
    !google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL
    vcode = getpass.getpass()
    !echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}

    ```



2. 去网页确认一下：

     > confirm the authentication of this access. Follow the order saying open the link, and you will visit some blue-ish page displaying your key. * 
3. 再挂载

    ```
    !mkdir drive
    !google-drive-ocamlfuse drive
    !ls drive/"Colab Notebooks"

    ```

就可以了。