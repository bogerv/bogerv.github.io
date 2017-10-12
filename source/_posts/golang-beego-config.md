---
title: beego Config 说明
date: 2016-10-19 17:25:50
categories: [go]
---

* beego.BConfig
    * AppName 
    * RunMode 
    * RouterCaseSensitive 
    * ServerName 
    * RecoverPanic 
    * CopyRequestBody 
    * EnableGzip 
    * MaxMemory 
    * EnableErrorsShow 
    * **WebConfig**
        * AutoRender 
        * EnableDocs 
        * FlashName 
        * FlashSeperator 
        * DirectoryIndex 
        * StaticDir 
            > 单目录: StaticDir = download

            > 多目录: StaticDir = download:down download2:down2

        * StaticExtensionsToGzip
            > StaticExtensionsToGzip = .css, .js

        * TemplateLeft
            > 模板左标签，默认值是{% raw %}{{{% endraw %}

            > beego.BConfig.WebConfig.TemplateLeft="{% raw %}{{{% endraw %}"

        * TemplateRight
            > 模板右标签，默认值是{% raw %}}}{% endraw %}

            > beego.BConfig.WebConfig.TemplateRight="{% raw %}{{{% endraw %}"

        * ViewsPath
        * EnableXSRF
        * XSRFKEY
        * XSRFExpire
        * **Session**
            * SessionOn
            * SessionProvider
            * SessionName
            * SessionGCMaxLifetime
            * SessionProviderConfig
            * SessionCookieLifeTime
            * SessionAutoSetCookie
            * SessionDomain
    * **Listen**

        * Graceful
        * ServerTimeOut
        * ListenTCP4
        * EnableHTTP
        * HTTPAddr
        * HTTPPort
        * EnableHTTPS
        * HTTPSAddr
        * HTTPSPort
        * HTTPSCertFile
        * HTTPSKeyFile
        * EnableAdmin
        * AdminAddr
        * AdminPort
        * EnableFcgi
        * EnableStdIo
    * **Log**
        * AccessLogs
        * FileLineNum
        * Outputs