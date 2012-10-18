---
layout: post
title: Embedding fonts in Flex apps
alias: /post/470852431/embedding-fonts-in-flex-apps
---
Recently I’ve been developing a Flex application at work using Adobe’s free
Flex SDK. If you want to be able to draw transparent text labels in Flex, you
need to have the fonts embedded in the application. Embedding a font is as
simple as including it in a style block in the mxml file for the application:

{% highlight xml %}
<mx:Style>
    @font-face
    {
        font-family: DejaVuSansMonoEmbed;
        src: url("../assets/DejaVuSansMono.ttf");
        unicode-range:
            U+0020-U+0040, /* Punctuation, Numbers */
            U+0041-U+005A, /* Upper-Case A-Z */
            U+005B-U+0060, /* Punctuation and Symbols */
            U+0061-U+007A, /* Lower-Case a-z */
            U+007B-U+007E; /* Punctuation and Symbols */
    }
    .mytextstyle
    {
        font-family: DejaVuSansMonoEmbed;
        font-size: 12pt;
    }
</mx:Style>
{% endhighlight %}

However, the next bit gets a bit freaky. The compiler will invariably fail to
build the application and give you an error that’s something like:

    [mxmlc] app.mxml(14): Error: exception during transcoding: Unexpected exception encountered while reading font file 'assets/DejaVuSansMono.ttf'
    [mxmlc] 
    [mxmlc]             font-family: DejaVuSansMonoEmbed;
    [mxmlc] 
    [mxmlc] app.mxml(14): Error: unable to build font 'DejaVuSansMonoEmbed'
    [mxmlc] 
    [mxmlc]             font-family: DejaVuSansMonoEmbed;
    [mxmlc] 
    [mxmlc] app.mxml(14): Error: Unable to transcode assets/DejaVuSansMono.ttf.
    [mxmlc] 
    [mxmlc] 
    [mxmlc] 

It turns out that the Flex compiler has a choice of font encoding engines to
use and the first one it tries is usually a bit rubbish. In order to avoid the
error, you need to force it to use the closed-source Adobe implementation
(which is shipped with the *free* Flex SDK, but not with the *open-source* Flex
SDK). At the command line, you need to add the option 
`-managers flash.fonts.AFEFontManager` to your mxlmc command.

If you’re using ant for your builds, you can add a font element to your mxmlc
block to do the same.

{% highlight xml %}
<target name="main">
    <mxmlc file="src/app.mxml"
           output="${BUILD_DIR}/bin/app.swf"
           incremental="true"
           debug="false">
        <fonts>;
            <manager>flash.fonts.AFEFontManager</manager>
        </fonts>
    </mxmlc>
</target>
{% endhighlight %}

(Yes, I know, I’ve not done any ANPR stuff in a while. I’ll get around to it.
Promise)
