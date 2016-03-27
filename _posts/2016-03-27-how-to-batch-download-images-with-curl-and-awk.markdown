---
published: true
title: How To Batch Download Images With Curl And Awk
layout: post
---
### Preparation
At first, let's have a CSV file which has image names and image URLs like below.

`urls.txt`

```
image_name,http://example.com/cat.png
image_name2,http://example.com/cat2.png
image_name3,http://example.com/cat3.png
image_name4,http://example.com/cat4.png
```

### Here Oneliner Command

#### For mac user

```
awk -F',' '{system("curl -S -# -o " $1 " " $2) }' urls.txt
```

#### For window user

`note`
You need to install [gawk](http://gnuwin32.sourceforge.net/packages/gawk.htm) and [curl](https://curl.haxx.se/latest.cgi?curl=win64-ssl-sspi).

```
gawk.exe -F"," "{system(\"curl.exe -S -# -o \" $1 \" \" $2) }" urls.txt
```

### The Options

#### -S
When used with -s it makes curl show an error message if it fails.

#### ♯
Make curl display progress as a simple progress bar instead of the standard, more informational, meter.

#### -o
Write output to <file> instead of stdout. If you are using {} or [] to fetch  multiple  documents,  you
can  use '#' followed by a number in the <file> specifier. That variable will be replaced with the current string for the URL being fetched.

### Note
- You need to escape the meta character in the image names or image URLs when you have.