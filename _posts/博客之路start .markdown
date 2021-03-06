---
layout: post_ymy
title: 博客之路start
date: '2016-01-23 01:17:00'
tags: [daily,first]
---

虽然我的工作是程序员，但是最近半年其实我的主要干的事儿是养了一个小孩。 所以这半年来可以说没有积累到什么技术，反而是积累了不少养小孩的心得。 当知道了有这么次会议可以分享这半年来的心得的时候，我毫不犹豫地选定了主题。那就是

如何打造一个让人愉快的小孩
但考虑到这是一次开发者会议...当我把这个想法和题目提交给大会的时候，被残酷地拒绝了。考虑到我们是一次开发者大会，所以我需要找一些更合适的主题。其实如果你对自己的代码有感情的话，我们开发和维护的项目或者框架就如同自己的孩子一般这也是我所能找到的两者的共同点。所以，我将原来拟定的主题换了两个字：



![有帮助的截图](/assets/images/profile.jpg)
你可以直接 [下载 PDF]({{ site.url }}/assets/mydoc.pdf)
{% highlight objc linenos %}
func weatherViewControllerForDay(day: WeatherViewController.Day) -> UIViewController {

    let vc = storyboard?.instantiateViewControllerWithIdentifier("WeatherViewController") as! WeatherViewController
    let nav = UINavigationController(rootViewController: vc)
    vc.day = day

    return nav
}
{% endhighlight %}

```
    if (currentClass) {
        unsigned int methodCount;
        Method *methodList = class_copyMethodList(currentClass, &methodCount);
        IMP lastImp = NULL;
        SEL lastSel = NULL;
        for (NSInteger i = 0; i < methodCount; i++) {
            Method method = methodList[i];
            NSString *methodName = [NSString stringWithCString:sel_getName(method_getName(method)) encoding:NSUTF8StringEncoding];
            if ([@"printTest" isEqualToString:methodName]) {
                lastImp = method_getImplementation(method);
                lastSel = method_getName(method);
            }
        }
        typedef void (*fn)(id,SEL);
        
        if (lastImp != NULL) {
            fn f = (fn)lastImp;
            f(test,lastSel);
        }
        free(methodList);
    }
```
<blockquote>
    <p>
这是我在今年 1 月 10 日 @Swift 开发者大会 上演讲的文字稿。相关的视频还在制作中，没有到现场的朋友可以通过这个文字稿了解到这个 session 的内容。
    </p>
</blockquote>


虽然我的工作是程序员，但是最近半年其实我的主要干的事儿是养了一个小孩。 所以这半年来可以说没有积累到什么技术，反而是积累了不少养小孩的心得。 当知道了有这么次会议可以分享这半年来的心得的时候，我毫不犹豫地选定了主题。那就是

如何打造一个让人愉快的小孩
但考虑到这是一次开发者会议...当我把这个想法和题目提交给大会的时候，被残酷地拒绝了。考虑到我们是一次开发者大会，所以我需要找一些更合适的主题。其实如果你对自己的代码有感情的话，我们开发和维护的项目或者框架就如同自己的孩子一般这也是我所能找到的两者的共同点。所以，我将原来拟定的主题换了两个字：

如何打造一个让人愉快的框架
在正式开始前，我想先给大家分享一个故事。我们那儿的 iOS 开发小组里有一个叫做武田君的人，他的代码写得不错，做事也非常严谨，可以说是楷模般的员工。但是他有一个致命的弱点 -- 喜欢自己发明轮子。他出于本能地抗拒在代码中使用第三方框架，所以接到开发任务以后他一般都要花比其他小伙伴更多的时间才能完成。

武田君其实在各个方面都有建树...比如

网络请求
模型解析
导航效果
视图动画 ...
不过虽然造了很多轮子，但是代码的重用比较糟糕，耦合严重。在新项目中使用的话，只能复制粘贴，然后针对项目修修补补。因为承担的任务总是没有办法完成，他一直是项目deadline的决定者，在日本这种社会，压力可想而知。就在我这次回国之前，武田君来向我借了一本我本科时候最喜欢的书。就是这本：



我有时候就想，到底是什么让一个开发者面临如此大的精神压力，我们有什么办法来缓解这种压力。在我们有限的开发生涯中，应该如何有效利用时间来做一些更有价值的事情。

显然，我们不可能一天建成罗马，也不可能一个人建成罗马。我们需要一些方法把自己和别人写的代码组织起来，高效地利用，并以此为基础构建软件。这就涉及到使用和维护框架。如何利用框架迅速构建应用，以及在开发和发布一个框架的时候应该注意一些什么，这是我今天想讲的主题。当然，为了让大家安心和专注于今天的内容，而不是挂念武田君的命运，特此声明：

以上故事纯属虚构，如有雷同实属巧合
使用框架

在了解如何制作框架之前，先让我们看看如何使用框架。可以说，如果你想成为一个框架的提供者，首先你必须是一个优秀的使用者。

在 iOS 开发的早期，使用框架其实并不是一件让人愉悦的事情。可能有几年经验的开发者都有这样的体会，那就是：

忘不了 那些年，被手动引用和 .a 文件所支配的恐惧
其实恐惧源于未知，回想一下，当我们刚接触软件开发的时候，懵懵懂懂地引用了一个静态库，然后面对一排排编译器报错时候手足无措的绝望。但是当我们了解了静态库的话，我们就能克服这种恐惧了。

什么是静态库 (Static Library)
所谓静态库，或者说 .a 文件，就是一系列从源码编译的目标文件的集合。它是你的源码的实现所对应的二进制。配合上公共的 .h 文件，我们可以获取到 .a 中暴露的方法或者成员等。在最后编译 app 的时候.a 将被链接到最终的可执行文件中，之后每次都随着app的可执行二进制文件一同加载，你不能控制加载的方式和时机，所以称为静态库。

在 iOS 8 之前，iOS 只支持以静态库的方式来使用第三方的代码。

什么是动态框架 (Dynamic Framework)
与静态相对应的当然是动态。我们每天使用的 iOS 系统的框架是以 .framework 结尾的，它们就是动态框架。

Framework 其实是一个 bundle，或者说是一个特殊的文件夹。系统的 framework 是存在于系统内部，而不会打包进 app 中。app 的启动的时候会检查所需要的动态框架是否已经加载。像 UIKit 之类的常用系统框架一般已经在内存中，就不需要再次加载，这可以保证 app 启动速度。相比静态库，framework 是自包含的，你不需要关心头文件位置等，使用起来很方便。

Universal Framework
iOS 8 之前也有一些第三方库提供 .framework 文件，但是它们实质上都是静态库，只不过通过一些方法进行了包装，相比传统的 .a 要好用一些。像是原来的 Dropbox 和 Facebook 等都使用这种方法来提供 SDK。不过因为已经脱离时代，所以在此略过不说。有兴趣和需要的朋友可以参看一下这里和这里。
title: 
I have really enjoyed the simplicity of the [Ghost](https://github.com/tryghost/Ghost" target="_blank) blogging system and wanted to use it for my personal site.  But I also enjoyed the ease of hosting with [GitHub Pages](https://pages.github.com" target="_blank) so I needed to find a way to generate a static version of a Ghost blog.  Luckily someone already had a handy little tool for the job called [Buster](https://github.com/axitkhurana/buster" target="_blank).


### Installing Buster

Details on Buster can be found [here](https://github.com/axitkhurana/buster" target="_blank). Basically, you need to install **wget** and **git** with [brew](http://brew.sh/" target="_blank) (if they are not already installed on your system) then install Buster with [pip](https://pip.pypa.io" target="_blank).  
Just run the following commands:

{% highlight bash %}
brew install wget
brew install git
pip install buster
{% endhighlight %}

To test the install of Buster run the following:

{% highlight bash %}
buster --version
{% endhighlight %}
You should see current version display (i.e. ```0.1.3```).


### Setting up Ghost

First off you need to download of a copy of the latest version of Ghost found [here](https://github.com/TryGhost/Ghost/releases" target="_blank). Extract the archive and open a console and go to the newly created directory and run the following:

{% highlight bash %}
npm install --production
npm start
{% endhighlight %}

Then visit http://127.0.0.1:2368/ghost in a browser and go through the setup screens to create an admin account and start using ghost.


### Setting up Buster

In a new console window go to the directory where ghost is installed and type the following to setup buster:

{% highlight bash %}
buster setup
{% endhighlight %}

Just follow the prompts and provide the data needed. Please view the README on the [Buster GitHub page](https://github.com/axitkhurana/buster" target="_blank) for more details on how to use Buster.


### Generating static pages with Buster

At the time of writing this Buster had some issues with missing the generation of some needed files and creating some files with incorrect URL reference in page. I created this script as a workaround for those issue.  So create a file (I called it **build-static.sh**) and be sure and change out the references to http://joshgerdes.com and replace them with your remote site URL.

{% highlight bash %}
#!/bin/bash

# Generate static files with buster
buster generate --domain=http://127.0.0.1:2368  

# Copy sitemap files
wget --convert-links --page-requisites --no-parent --directory-prefix static --no-host-directories --restrict-file-name=unix http://127.0.0.1:2368/sitemap.xsl
wget --convert-links --page-requisites --no-parent --directory-prefix static --no-host-directories --restrict-file-name=unix http://127.0.0.1:2368/sitemap.xml
wget --convert-links --page-requisites --no-parent --directory-prefix static --no-host-directories --restrict-file-name=unix http://localhost:2368/sitemap-pages.xml
wget --convert-links --page-requisites --no-parent --directory-prefix static --no-host-directories --restrict-file-name=unix http://localhost:2368/sitemap-posts.xml
wget --convert-links --page-requisites --no-parent --directory-prefix static --no-host-directories --restrict-file-name=unix http://localhost:2368/sitemap-authors.xml
wget --convert-links --page-requisites --no-parent --directory-prefix static --no-host-directories --restrict-file-name=unix http://localhost:2368/sitemap-tags.xml

# Replace urls that were missed by buster
find static/* -name robots.txt -type f -exec sed -i '' 's#http://localhost:2368#http://joshgerdes.com#g' {} \;
find static/* -name *.xsl -type f -exec sed -i '' 's#http://localhost:2368#http://joshgerdes.com#g' {} \;
find static/* -name *.xml -type f -exec sed -i '' 's#loc>http://localhost:2368#loc>http://joshgerdes.com#g' {} \;
find static/* -name *.html -type f -exec sed -i '' 's#u=http://localhost:2368#u=http://joshgerdes.com#g' {} \;  
find static/* -name *.html -type f -exec sed -i '' 's#url=http://localhost:2368#url=http://joshgerdes.com#g' {} \;  
find static/* -name *.html -type f -exec sed -i '' 's#href="http://localhost:2368#href="http://joshgerdes.com#g' {} \;  
find static/* -name *.html -type f -exec sed -i '' 's#src="http://localhost:2368#src="http://joshgerdes.com#g' {} \;  
find static/* -name *.html -type f -exec sed -i '' 's#link>http://localhost:2368#link>http://joshgerdes.com#g' {} \; 

# Add CNAME file for github pages
buster add-domain joshgerdes.com

# Copy files that were missed by buster
cp humans.txt static/humans.txt
cp -R content/images static/content
{% endhighlight %}

Now give the newly created file execute permissions 

{% highlight bash %}
chmod +x ./build-static.sh
{% endhighlight %}

Then just run in the directory where you installed ghost:

{% highlight bash %}
./build-static.sh
{% endhighlight %}

This should generate static files under a subdirectory called ```/static``` which should be connected to your GitHub repository.

***NOTE**: I also forked the Buster repository and made changes to the project to fix the broken URLs and add the sitemap files to the static output.  So if you want to use my version of Buster it can be found [here](https://github.com/joshgerdes/buster" target="_blank). Hopefully my pull request will be accepted shortly and it will become part of the main script.*


### Previewing static pages

You can preview the static pages by running the following command:

{% highlight bash %}
buster preview
{% endhighlight %}

Then visit http://localhost:9000 in a browser to view the static pages locally.


### Deploying static pages to GitHub

If you like what you see, you can push those changes to your GitHub repository using normal commands or type the following command:

{% highlight bash %}
buster deploy
{% endhighlight %}

With luck you should have a nice static version of your site published to GitHub to enjoy.

