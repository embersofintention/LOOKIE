



...or dysfunction notes more accurately 

---
# How It Basically Works

* Content managed in Obsidian vault, LOOKIE (this)
* Push to GitHub
* Repo on Github fires off an Action that connects with the Site repo, LOOKIESITE; updates a submodule with most recent LOOKIE vault content
* an Action in the LOOKIESITE repo uses Kiln, a static site generator, to create a website and post to GitHub Pages

---







# Highlights, key notes, major discoveries


# Syncing to Github:
---

## Gitless Sync Will Not Work, Do Not Use!
* Gitless Sync has a few game breaking issues that will make it NOT workable for this project.  (2/19/2026, 4:17pm)
	* you cannot delete files or sync won't work
	* links created on a page do not work on the synced thing
	* if you rename a file, you wind up with both versions 

## Git Plugin Works! (2/27/26, 5:42pm) 
* None of the Gitless Sync issues are present.  Works much better for my purposes here.
* I'll just have to **find a way to remotely trigger commits to the LOOKIE vault from my desktop, using mobile** so I can post stuff from my phone




# Syncing between Devices:
---
### Do Not Rely on Github for Syncing Between Devices
* otherwise local changes would get overwritten and that would be bad
* For now, I can just use Syncthing...
* Note:  I can worry about device syncing after everything else is working










---

quirks: When using Gitless Sync to update Vault

* images don't seem to work unless I rename them in obsidian after adding them.  No idea why. 
* site seems to appear broken for a minute or so after it updates, with the layout jumbled.  Refreshing after a minute seems to fix it though. 
* sync seems to crash if any files are deleted from the vault.  I'll have to look into this. 

**UPDATE** 2/19/2026, 4:17pm 
* Gitless Sync has a few game breaking issues that will make it NOT workable for this project.  




---
# Testing methods of embedding stuff

---

## Images
I dragged and dropped each of these from the "attachments" folder in the vault, right here within obsidian.  Each was done (and synced to the site) when the vault's settings were set to use its type of link.  (tested while using Gitless Sync)

![[ralien sticker weirdface.png]] 
-- This one uses "shortest link when possible" 


---


![[attachments/ralien sticker weirdface.png]] 
This one uses -- "from vault folder"

**This one doesn't seem to work** so it seems we need to use "path from shortest link possible"

---


## Why Do Links Not Work On Site?
trying to test this now...

[[index]]
* as of 3:53pm on 2/19/26, this doesn't do anything when you click it.  Boo.  
	* Theory:  possibly because Kiln generates the site from a submodule??
		* except the *images* work, which is odd.  
		* Though I have to rename them in obsidian before they do, or they render broken. 
	* Thing to Try:  Make a page. Rename it. Sync.  
		* [[test 2-27-26 530pm]]
		* ^ linked without renaming. Doesn't work. 
		* now I'mma rename it something and try again..
		* [[test 2-27-26 530pm]]   -- it auto updated my previous link, but I draggydropped it again just in case there's a difference.  
		* Interesting.. on site, BOTH versions actually show up
	* **ISSUE FOUND:  it seems like either Gitless Sync or Kiln don't like when a file is renamed or deleted??
		* it seems to be Gitless Sync, because this shows up in the vault repo that way too. :/
	* **ISSUE FOUND**  I discovered that once synced, the links don't work as links within the Vault repo either.  So the issue is Gitless Sync.  

### Testing:  Links in local vault don't do anything when synced (2/19/26, 4:28pm )

* Thing to Try:  make a link by manually copying path... 
	* as Obsidian URL  - obsidian://open?vault=LOOKIE&file=test%20bs -- 
* Discovery:   Won't work! Here's why..
	* links are created by putting the file name in double brackets. 
	* if you put a path in the brackets, it'll think that's the file name

CONCLUSION:  
* since switching git sync methods will likely stir things up a bunch anyway, I'm not gonna worry about it. 
* If it's still an issue using a different sync method, I'll revisit. 


## Brainstorming:  Other methods to push to Github

Important notes:
* I will need to use a different thing to sync between desktop and mobile.  Github is ONLY for publishing to the web page thing.  
	* Syncthing?
* it will need to be able to PUSH ONLY, without pulling!!
	* don't want local changes overwritten by what's on the site. 

If I use Syncthing to keep my vault synced between mobile and desktop, maybe I can just find a way to have git commit and push my changes automatically from my pc..?
* 


### Trying GitSync -- 2/27/2026, 4:40pm
I'm gonna try out this method:  https://viscouspotenti.al/posts/gitsync-all-devices-tutorial

* I've made a branch on LOOKIESITE called "main backup 1" so that in case this breaks things, I can revert back to where it was before.  


5:31pm 
* reconnected the vault with Git and GitHub
* did a commit from the terminal (I'm so proud of myself), using directions from here: https://obsidian-bloger.pages.dev/git-setup-for-obsidian/
* ~~there's apparently a Pages outage though, so the deploy failed.  Gotta try again later. 
* ~~In the meantime though, I can make sure the plugin works by committing the changes I made here, adding these notes
* Update:  ran the action manually from github.com.  Worked!  Site generated just fine, AND no more duplicate note issue :D :D :D

#### What I Learned: 
* **perform github syncing with the Git plugin, it works without any of the issues I had with Gitless Sync


**Next Issue to Troubleshoot:  having Kiln generate working links**


## Troubleshooting Links on Kiln Site (2/27/2026, 5:47pm)

### Issue: 
* The links on generated site don't do anything

### Theory 1:  Kiln may not know how to generate links between pages in a submodule
*First, let's rule out that the issue was with our method of syncing to github... 
* Test: 
	* To do this, let's create a link (now that we're using that Git plugin instead of Gitless Sync), and see if it breaks on the site:   [[test 2-27-26 530pm]] 
* Results:  
	* Link doesn't go anywhere still.  
	* Also when you visit this page, there's a white bar on the bottom of the screen for some reason now and the scroll is borked?
* Conclusions: 
	* The issue with links is not from the sync method.  That's successfully ruled out.
* Next Steps: 
	* do another commit, and see if white bar weirdness issue resolves itself
	* if not, try deleting that test link and try again

#### *new issue arises:* Weird white bar and scroll issues on this page (2/27, 6:54pm)
*Theory 1:  Happens when a page gets too long?
* Test:  
	1. back up content of this page, so I can paste it back easy enough
	2. delete everything but a couple words or whatever
	3. commit, push, let site build 
	4. ????
	5. profit
* Results: 
	* the content being directly copy-pasted onto this page work just fine, so it's not the length...
	* Site Function Notes page doesn't have the white bar now though? 
	* Sooo.. who knows.
* Next:  
	* paste content back onto Site Functions Notes page, and see if it breaks again
* Results: 
	* now THIS page has the issue????
* Next thing to try:
	* Since I'm no longer using Gitless Sync, I don't need the submodule thing.  
	* Maybe I can just ass-blast this thing into one vault, and solve a whole lot of issues at once. 


> [!info] To Be Continued:  Recreating all this jazz in one vault
> * copy deploy.yml, take out the submodule stuff
> * see what happens I guess?  
> ...I gotta stop for now, I went way longer than I planned to today (it's 7:30pm now).   I can pick back up with the documenting and experimenting next time.












.












.