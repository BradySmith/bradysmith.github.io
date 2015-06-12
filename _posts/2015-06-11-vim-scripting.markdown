---
layout: post
title:  "Vim Functions" 
date:   2015-06-11 23:45:10
categories: 
---
One of my programming guilty pleasures is [Vim](http://www.Vim.org/). While there is much discussion around whether or not Vim actually makes you more productive, I like using it for the simple fact that I find it __fun__ use. Now that I've used it for a few years, I think I'm finally at the point where I can code in Vim faster than I would be able to in something like Sublime or Atom. I'm sure that says something about the productivity argument even if I'm not sure what. Maybe in another year or two I'll feel like I have something to add to that discussion.

There are two things I love about "the Vim experience" that I haven't been able to find in a "normal" text editor. First is it's customizability. When I first started learn Vim it was recommended that I start by reading [Learn Vimscript the Hard Way](http://learnVimscriptthehardway.stevelosh.com/). One of the things Steve recommends in his book is to set up a custom `.vimrc` file and fill it with your own custom keybinds and shortcuts which then give you a totally custom built editor. I've been doing that for two years now and have a file that is pretty personal and makes it so when I use Vim - it's _my Vim_. You can check my [vimrc out on Github](https://github.com/BradySmith/dotVim/).

The second thing that I love about Vim is it's depth. Since my initial read-through of Learn Vimscript the Hard Way, I've read it several times through and each time through I'll find something I'd missed previously. It blows my mind that I've been using Vim daily for school and work for about two years now and I'm still finding out new things.

Case in point last night I wrote my first function:

{% highlight vim %}
function! BradyFlip()
    let wordUnderCursor = expand("<cword>")
    let pairs = ["TRUE", "FALSE", "Good", "Bad", "0", "1", "+", "-"]
    let i = 0

    while i < len(pairs)
        let queryWord = get(pairs, i) 
        
        if wordUnderCursor ==# queryWord
            let save_cursor = getpos(".")

            if (i % 2 == 0)
                let targetWord = get(pairs, i+1)
            else
                let targetWord = get(pairs, i-1)
            endif

            put =targetWord
            normal diw
            call setpos('.', save_cursor)
            normal viwp
            normal <esc>
            normal jdd
            call setpos('.', save_cursor)
        endif

        let i += 1
    endwhile
endfunction
{% endhighlight %}

I'll admit - it's more than a little crude not all of which can be blamed on Vim's archaic scripting language. The gist of what the function is doing is looking at the word under the cursor and checking to see if it has been defined in the function's `pairs` array. If the word is found, the function swaps it with the word's pair: `0` being replaced by `1`, `true` being replaced by `false` and vice versa.

Interestingly the bottom half of the function is my hacky solution to the dilemma of trying to get Vim to insert a variable at the cursor as using `<C-R>=variable` wasn't working in `normal` mode from a script. Feel free to shoot me an email or open an issue on my [Github](https://github.com/BradySmith/dotVim/) if you know a good refactor for that particular problem.

As far as better solutions go: I'm sure there's an already made Vim plugin that does this better than the way I've implemented it here. However, much for the same reasons that I have my own Vimrc instead of using something like [spf13](https://github.com/spf13/spf13-Vim) - sometimes there's no better way to learn than to do it yourself.
