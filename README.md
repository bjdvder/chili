Chili
=====
Dropbox powered static site generator.

# Install
```bash
$ git clone git://github.com/clvrobj/chili.git
$ cd chili
$ pip install -r requirements.txt
```

## Dropbox configuration
* [Create an Dropbox app](https://www.dropbox.com/developers/apps), `App name` → `YourAppName`, `Access level` → `App folder` , Dropbox will generate `App key` and `App secret` for you, a folder named `YourAppName` should be created in your Dropbox folder automatically.
* In your Dropbox folder `~user/Dropbox/Apps/YourAppName`, write article with Markdown and save with `.md`or .markdown suffix.

## Configuration
You can configure Chili, by modifying the `/config.py` file.

* `DROPBOX_APP_KEY`: `App key` of your app in Dropbox
* `DROPBOX_APP_SECRET`: `App secret` of your app in Dropbox
* `DROPBOX_ACCOUNT_EMAIL`: Dropbox account email address (lock to this account , other account will not be sync)
* `APP_SECRET_KEY`: for using flask session, this value can be get by

``` python
>>> import os
>>> os.urandom(24)
```

* `DOMAIN`: your domain name, like `chilipy.com`
* `BLOG_NAME`: shown at the top of site.
* `DISQUS_SHORTNAME`: use Disqus for comments.
* `TIMEZONE`: set your timezone.
* `NAV_ITEMS`: items shown in nav menu.
* `TRACKING_CODE`: you can insert script snippet to the bottom of the page.

Here the default:

``` bash
DROPBOX_APP_KEY = ''
DROPBOX_APP_SECRET = ''
DROPBOX_ACCOUNT_EMAIL = ''
APP_SECRET_KEY = ''

DOMAIN = 'yourdomain.com'
BLOG_NAME = 'Site title'
TWITTER_NAME = ''
DISQUS_SHORTNAME = ''
TIMEZONE = ''
NAV_ITEMS = ('about', ) # items shown in nav menu. 
TRACKING_CODE = """
<script type="text/javascript">
</script>
"""
```

## Deployment
### Nginx configuration
You can configure the nginx like this:

``` bash
server {
    listen 80;
    server_name you-domain;
    location / {
      fastcgi_pass  127.0.0.1:7777;
      root /var/path-to-chili/;
      fastcgi_param REQUEST_URI       $request_uri;
      fastcgi_param REQUEST_METHOD    $request_method;
      fastcgi_param QUERY_STRING      $query_string;
      fastcgi_param CONTENT_TYPE      $content_type;
      fastcgi_param CONTENT_LENGTH    $content_length;
      fastcgi_param SERVER_ADDR       $server_addr;
      fastcgi_param SERVER_PORT       $server_port;
      fastcgi_param SERVER_NAME       $server_name;
      fastcgi_param SERVER_PROTOCOL   $server_protocol;
      fastcgi_param PATH_INFO         $fastcgi_script_name;
      fastcgi_param REMOTE_ADDR       $remote_addr;
      fastcgi_param REMOTE_PORT       $remote_port;
      fastcgi_pass_header Authorization;
      fastcgi_intercept_errors off;
    }
}
```


### Start Chili on server
* `sh restartapp.sh`

# Writing Posts
Write article with Markdown and save with .md`or .markdown suffix in your Dropbox folder `~user/Dropbox/Apps/YourAppName`.

## How to sync new post?
* Just request `http://your-domaindotcom/sync` in browser,  this will sync Dropbox app folder and download all posts, all your posts will be generated,  you will be asked for the permission for accessing Dropbox Chili folder for the first time.
* If request `http://your-domaindotcom/regen` in browser, this will generate all your posts without sync, this is useful when you just make some customize on the template.

All these links are list on`http://your-domaindotcom/tools`.

## How to embed image?
Create directory `image` in `~user/Dropbox/Apps/YourAppName`, put image file into it, then write like this in the post:

`![image title](/img/filename.jpg)`

## About Markdown meta data
You can control the post by using the meta data:

* Title: title of the post
* Date: the create time of the post
* Public: public switch
* Comment: Chili using Disqus to support comments, comments will not shown if set to off.
* Keywords: the tags of the post

Example:

``` bash
Title: This is title
Date:  2012-09-24 09:26:00
Public: Yes
Comment: Yes
Keywords: markdown
          dropbox
          blog


[Post content]
```

# Testing
`python tests/chili_test.py`

# Example
ChiliPy.com: <http://chilipy.com/>
