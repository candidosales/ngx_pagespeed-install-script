## ngx_pagespeed-install-script
#Install nginx and pagespeed latest version on clean Ubuntu 14.04 x64

Need root right - sudo su

Installation

'wget https://github.com/Levantado/ngx_pagespeed-install-script/releases/download/1a/page-speed.sh ; bash page-speed.sh'

for 15.10 use

'https://github.com/Levantado/ngx_pagespeed-install-script/releases/download/1a/page-speed-1v1.sh ; bash page-speed-1v1.sh'

<h2>How to Install nginx and google pagespeed on Ubuntu 14.04 x64</h2>

<h3>What is it:</h3>

<blockquote>
<p><strong>Nginx</strong> is an HTTP and reverse proxy server, a mail proxy server, and a generic TCP proxy server, originally written by Igor Sysoev. For a long time, it has been running on many heavily loaded Russian sites including Yandex, Mail.Ru, VK, and Rambler. According to Netcraft, nginx served or proxied 22.61% busiest sites in August 2015. Here are some of the success stories: Netflix, Wordpress.com, FastMail.FM.<em>
</em></p>

<p><em><strong>Ngxpagespeed</strong> speeds up your site and reduces page load time by automatically applying web performance best practices to pages and associated assets (CSS, JavaScript, images) without requiring you to modify your existing content or workflow. Features include:</em></p>

<p><em>  • Image optimization: stripping meta-data, dynamic resizing, recompression</em></p>

<p><em>  • CSS &amp; JavaScript minification, concatenation, inlining, and outlining</em></p>

<p><em>  • Small resource inlining</em></p>

<p><em>  • Deferring image and JavaScript loading</em></p>

<p><em>  • HTML rewriting</em></p>

<p><em>  • Cache lifetime extension</em></p>

<p><em>  • and more</em></p>
</blockquote>

<h3>Prerequisites</h3>

<ul>
	<li>Ubuntu 14.04 x64 (may 15.05)</li>
	<li>root privileges</li>
	<li>few knowledge using vim</li>
</ul>

<h3><strong>Step 1 Install some Debian package development tools and library</strong></h3>

<pre><code>sudo apt-get install dpkg-dev build-essential zlib1g-dev libpcre3 libpcre3-dev unzip
</code></pre>

<h3><strong>Step 2 Adding nginx repo</strong></h3>

<pre><code>sudo nano /etc/apt/sources.list.d/nginx.list
</code></pre>

<p>Paste in this two string </p>

<pre><code>deb http://nginx.org/packages/ubuntu/ trusty nginx
deb-src http://nginx.org/packages/ubuntu/ trusty nginx
</code></pre>

<p>You may use vim that more helping because that have syntax highlighting. (In vim enable inserting mode press “i”, disabling all mode press “ESC”, exit without save “ZQ”, exit with save ”ZZ”);</p>

<p>This difference</p>

<p>nano:<img src="Screen%20Shot%202015-09-01%20at%2013.19.27.png"/></p>

<p>vim: <img src="Screen%20Shot%202015-09-01%20at%2013.19.49.png"/></p>

<pre><code>sudo apt-get update
</code></pre>

<p>Command stop and show strings like that:</p>

<p> <em>
</em></p>

<p><em><strong>GPG error: http://nginx.org trusty Release: The following signatures couldn&#39;t be verified because the public key is not available: NOPUBKEY ABF5BD8xxxxxxx</strong></em></p>

<p>Don’t worry add this key </p>

<p>￼</p>

<pre><code>sudo sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABF5BD8xxxxxxx￼
</code></pre>

<p>repeat</p>

<pre><code>sudo apt-get update
</code></pre>

<h3><strong>Step 3 Downloading and extracting</strong></h3>

<p><strong><em>Step 3.1 Download nginx</em></strong></p>

<p>Enter root mode</p>

<pre><code>sudo su
</code></pre>

<p>Prepare place</p>

<pre><code>cd ~
mkdir -p ~/new/nginx_source/
cd ~/new/nginx_source/
</code></pre>

<p>Downloading</p>

<pre><code>apt-get source nginx
apt-get build-dep nginx
</code></pre>

<p><strong><em>Step 3.2 Download pagespeed</em></strong></p>

<p>Prepare place</p>

<pre><code>cd ~
mkdir -p ~/new/ngx_pagespeed/
cd ~/new/ngx_pagespeed/
</code></pre>

<p><em>You may see this steps in official site <a href="https://developers.google.com/speed/pagespeed/module/build_ngx_pagespeed_from_source">https://developers.google.com/speed/pagespeed/module/build_ngx_pagespeed_from_source</a></em></p>

<p><strong>Downloading and unpacking</strong></p>

<p>Check ngx-version on the official site and change if you need. 
    <a href="https://developers.google.com/speed/pagespeed/module/release_notes"> https://developers.google.com/speed/pagespeed/module/release<em>notes </em></a>
    <img src="releases-ngx_page_speed.png"/>
</p>

<pre><code>ngx_version=1.10.33.6
wget https://github.com/pagespeed/ngx_pagespeed/archive/release-${ngx_version}-beta.zip
unzip release-${ngx_version}-beta.zip
cd ngx_pagespeed-release-${ngx_version}-beta/
wget https://dl.google.com/dl/page-speed/psol/${ngx_version}.tar.gz
tar -xzf ${ngx_version}.tar.gz
</code></pre>

<h3><strong>Step 4 Configure before build</strong></h3>

<pre><code>cd ~/new/nginx_source/nginx-1.8.1/debian/
</code></pre>

<p>Time use vim. That help avoid syntax errors</p>

<pre><code>vim rules
</code></pre>

<p>Need add in configure module that strings</p>

<pre><code>--add-module=../../ngxpagespeed/ngxpagespeed-release-1.10.33.6-beta \
</code></pre>

<p>after string <code>--prefix=/etc/nginx \
</code></p>

<p>in two section </p>

<p>first section: <code>ovveride_dh_auto_build: </code></p>

<p>second: <code>configure_debug:</code></p>

<p>If you see that highlights this string in vim different then other strings check “ \” and “—“ and set manually.</p>

<h3><strong>Step 5 Building</strong></h3>

<pre><code>cd ~/new/nginx_source/nginx-1.8.1/
dpkg-buildpackage -b
cd ~/new/ngix_source/
dpkg -i nginx_1.8.1-1~trusty_amd64.deb
</code></pre>

<p><strong><em>Step 5.1 Problem with compile CC</em></strong></p>
<pre><code>
    mount | grep tmp
    export PATH="/usr/bin:$PATH"
</code></pre>

<h3><strong>Step 6 Check</strong></h3>

<pre><code>nginx -V
</code></pre>

<p>in text we must see</p>

<figure><img src="version_nginx.png"/></figure>

<p>Now go to you public IP</p>

<p>You must see: </p>

<figure><img src="Screen%20Shot%202015-09-01%20at%2013.06.51.png"/></figure>

<h3><strong>Step 7 Config</strong></h3>

<p>Make cache folder</p>

<pre><code>mkdir -p /var/ngx_pagespeed_cache
chown -R www-data:www-data /var/ngx_pagespeed_cache
</code></pre>

<p>Enable pagespeed and cache</p>

<pre><code>vim  /etc/nginx/nginx.conf
</code></pre>

<p>In http section add this strings</p>

<pre><code>pagespeed on;
pagespeed FileCachePath /var/ngx_pagespeed_cache;
</code></pre>

<p>Like that</p>

<figure><img src="config_nginx_pagespeed.png"/></figure>

<p>Restart</p>

<pre><code>service nginx restart
</code></pre>

<p>Check </p>

<pre><code>curl -I -p http://localhost | grep X-Page-Speed
</code></pre>

<p>You must see </p>

<figure><img src="result_pagespeed_curl_1v.png"/></figure>

<p>It’s all guys!</p>

<p>P.S. for configure like wanna you - go here :</p>

<p><a href="https://developers.google.com/speed/pagespeed/module/configuration">https://developers.google.com/speed/pagespeed/module/configuration</a></p>