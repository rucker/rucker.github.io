# Running Multiple Firefox Profiles on MacOS
I have a use-case where it makes sense to run two Firefox profiles concurrently on MacOS. Furthermore, I want to be able to launch each of them via Spotlight rather than running a script by hand.

Some cursory research on a solution was slightly disappointing: articles I found appeared to be either [cumbersome and possibly outdated](https://spf13.com/post/managing-multiple-firefox-profiles-in-os-x/) or [not particularly helpful](http://kb.mozillazine.org/Opening_a_new_instance_of_Firefox_with_another_profile) due to the way MacOS app launches work. The former relied on mucking with a plist file and the application icon; the latter is not a complete solution because on MacOS, we're dealing with a convention where the `-no-remote` argument isn't easily written into a .desktop file as on Linux, or an application shortcut as on Windows. 

The following is my solution to this problem.

I have two complete Firefox application directories -- each containing a unique profile -- living at:  
/Applications/Firefox.app/Contents/MacOS  
~/Applications/Firefox (Other).app/Contents/MacOS  
(the "Other" profile began as a simple copy of the original Firefox.app directory)

## Setup
- Copy your existing Firefox.app directory and give it a name that suits you (see my example above). Later on, you can always create a new profile in the second Firefox directory using the [Profile Manager](http://kb.mozillazine.org/Profile_Manager)
- Add the following script to each directory and name it firefox.sh:  

```
#!/bin/bash

PROFILE_NAME=your-profile-name-here
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
"$DIR/firefox-bin" --no-remote -P $PROFILE_NAME
```
- `$ cp firefox firefox.bak`
- `$ mv firefox firefox-bin`
- `$ cp firefox.sh firefox`

What happens when the application is launched through Spotlight is `firefox` (the shell script) is invoked. It then calls the actual firefox binary, `firefox-bin`, with the absolute directory path prepended.

Because each profile is contained in its own Firefox application directory, Spotlight sees them as separate applications that can be launched freely at any time:

![]({{ site.url }}/assets/multi-firefox.png)

## Firefox Updates
One caveat to bear in mind is that each time Firefox receives an update, your `firefox` script will be replaced by the new version of the Firefox binary. This means a little maintenance is required after each update. The following script should take care of that:

```
#!/bin/bash

FIREFOX_DIRS=(profile-dir-1 profile-dir-2)
for dir in ${FIREFOX_DIRS[@]}; do
  cd $dir
  cp firefox firefox.bak
  mv firefox firefox-bin
  cp firefox.sh firefox
done
```
Happy browsing!
