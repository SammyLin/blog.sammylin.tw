---
layout: post
title: "在vim裡強迫使用hjkl"
date: 2012-07-13 09:09
comments: true
categories: [vim]
---
其實我用vim的時間其實不長啦，應該也還不到一年的時間，以前都是用`Visual Studio`，真的是完全不習慣，雖然有[Sublime Text](http://www.sublimetext.com/2)或是[TextMate](http://macromates.com/)這些比較介面上漂亮的的東西，不過其實`vim`也可以在ssh到別的主機時，可以直接用`vim`來開code就好了，不用還要下載回來修改完又要上傳，而且vim其實有很多plugin可以用，也真的很方便，重點是帥氣啦。
好吧..回到主題

### 修改vimrc

進到 .vimrc 裡加上這四行

    map <Left> :echo "Use h you asshole!"<cr>
    map <Right> :echo "Use l you asshole!"<cr>
    map <Up> :echo "Use k you asshole!"<cr>
    map <Down> :echo "Use j you asshole!"<cr>

不過就是把上下左右鍵改成echo 就行了...
感謝[Jimmy](http://gogojimmy.net/)的教我..

