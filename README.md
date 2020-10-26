# AppleNotes2Joplin

I made this AppleScript because nothing else worked well enough, including [this](http://falcon.star-lord.me/exporter/).

I hope it's useful to help people leave the grips of the Apple ecosystem like I am. I'm happy to maintain this for the community for that reason alone. Long live Linux.

This has some very hacky code, but it does the job. It could be way cleaner, more efficient and have better UX if I really wanted to spend the time, but I saw no need to make the code unnecessarily elegant because I'm not a pro developer so it takes time for me to learn new code to get something done. I welcome PRs to clean this up, improve it, or make it more complete with HTML cleanup, conversion to Markdown, or copying over Apple's rich text formatting to simple in-line HTML ready for [Joplin](https://github.com/laurent22/joplin).

**This is a one-click AppleScript to export all your macOS Apple Notes directly into Joplin export format, preserving:**

- The title/heading/body of every note.
- Note creation and last modification timestamps. (Integrated into native Joplin note metadata so they are as if you created them in Joplin directly.)
- Your folder structure. (But not nesting, fix that later.) (Or manual folder ordering, which Joplin doesn't offer.)
- Most rich text formatting, within reason. (Including: rich text HTML links, bold/italics, bulleted lists up to 10 indent levels, etc. Tables are problematic. Copy a rich text Apple table into Typora, save that as MD then copy that markdown code into a Joplin note).
- Unicode characters above U+0100.

**Things not exported by this script:**

- Embedded images. (Solution: In Notes.app > View > Show Attachments Browser, you can locate notes with images, drag them out of a note, then drag them into a Joplin note.)

- Colour text. (This script strips out any HTML colour code.)

- Underlined text. (Markdown doesn't do underline. You could probably add to the script to convert Apple rich text underlines to simple in-line HTML underline code to preserve it in Joplin. I don't use it myself, so please submit a PR if you want to share.)

- Font size formatting in Apple. (I hardly used that. Use the `Raw HTML tool` commented in the script to see example of how you can manually move text size over to in-line HTML. Please submit a PR if you can add this ability, would be nice.)

- Numbered lists. (Sorry it's complicated and I don't use them myself. I welcome a PR to implement it alongside my bulleted list to markdown code.)

- Possibly other obscure formatting you might use in your Apple Notes. (E.g. subheading, heading, checklist, strike-through, text alignment, non-list indentation. Again, please add with a PR if you needed that and have some code to add.)

**Other imperfections:**

- Very occasionally, an Apple note can leave behind an incomplete HTML hyperlink tag for some reason (either just above or below the same link that is in tact). It converts to a half complete markdown link. Very rare, but I noticed it.

**Read this before you run the script:**

- All you need is macOS. (Tested on Catalina.) Nothing else. Not even Homebrew.
- To be safe, first backup your Apple notes (e.g. Time Machine or iCloud notes via an iPhone backup on the Mac). Low likelihood of risk  as the script never deletes Apple notes or does paste actions inside Notes.app - it's just select-all and copy.
- You may have to unlock any password-protected notes before exporting. (If you want privacy during this whole process, turn off Internet.)
- Note any Apple `Recently Deleted` notes. Simplest to clear them out first, or unselect that folder during export.
- I suggest you read through the AppleScript to understand what it does and customise it accordingly. E.g., if you specifically use titles in your Apple notes (and not just body text), there's a section to modify in the script to cater to that. If you need more than 10 levels of bulleted list nesting, you can add extra lines to accommodate that.
- This script exports notes per Notes account. By default it is the `iCloud` account. If need be change it to your situation (or to export for additional accounts), where you see `tell account "iCloud"` three times in the script.
- It takes about 1-3 seconds per note to process, which is 1-1.5 hours for ~2000 notes. Therefore, I suggest you do your exporting in batches, selecting only only some folders each time. Choose the same export folder for each batch to let them accumulate together for a single Joplin import action which will import all folders and notes at once.
- To avoid a `Notes got an error: Can’t get note * of folder "*" of account "*". Invalid index` error that I noticed - a UX bug in Apple Notes - first do this: Set Notes.app to list the entirety of your notes (e.g. open `All iCloud` folder), select then top note, and hold down the arrow key to zoom through all notes. It will fix any 'phantom' leftover empty notes and delete them as it should have. Watch the note count to see if it reduced any of those half-dead notes.
- Do not turn the screen off during the script process. It needs screen on to do the macro stuff. You can keep the screen alive using [Amphetamine](https://roaringapps.com/app/amphetamine).

**How to use:**

- Download and extract `AppleNotes2Joplin.applescript` via the ZIP file [here](https://github.com/mindfulsource/AppleNotes2Joplin/archive/main.zip). (Do NOT copy the code from the raw contents on github and put it into `Script Editor.app`. Its code is somehow mangled and it won't work.) Press 'Save' in Script Editor to make the code more readable.
- Execute the script. Process is self-explanatory. It will slowly sift through your notes like a macro. Don't touch the computer while it does this. To stop it, jump in at any time, switch to Script Editor, and press the "Stop" button.
- If you have any individually huge  Apple notes: according to [this](https://apple.stackexchange.com/a/94756/163629), there may be a limitation per note of the byte size that the command `getconf ARG_MAX` returns in your Terminal (e.g. 262.14 kB). If the script seems to completely stop on a large note, give it five minutes or even ten minutes, otherwise close off the script file, break up the massive Apple note into smaller ones, then re-execute the script. (Try not to create duplicate notes, e.g. split where you left off into a new folder). None of my 2000 notes failed, my largest was 136kB and it took a few minutes to process before moving on to the next note.
- You can now import all your notes in one single action via Joplin's File > Import > "RAW - Joplin Export Directory". Select the single export folder you chose during the AppleScript and all your folders with notes will import into Joplin, not not folder nesting. Drag folders into other ones to emulate your nesting.
- Inspect your imported notes in Joplin. See if it exported all notes by comparing numbers of notes per folder. Manually fix and drag across anything incomplete such as embedded media. Some rich text or other elements can carry over nicely if you manually paste in either Joplin's code edit mode, WYSIWYG edit mode, or best, external editor linked from Joplin such as [Mark Text](https://github.com/marktext/marktext).

**Useful things I noted:**

- This script exports your pinned Apple notes just fine. But Joplin doesn't (at least yet) have a pinned notes feature. You'll have to find a different way to make them prominent in Joplin.
- A lot of HTML code appears to render in Joplin's WYSIWYG editor. You can add new formatting that you couldn't do before in Apple Notes, like in-line text background colour. This is an upgrade!
- Joplin notes' timestamps (in the raw data) are in UTC (GMT). I'm not sure if this presents problems for someone having been in more than one timezone with Apple's system, but if you've only been in one timezone with previous Apple notes it will carry over accurately via this script. I have it convert your Apple timestamps to pure GMT in the raw Joplin notes data.
- If you set up Joplin's encryption in its settings before importing your Apple notes, any cloud that syncs with Joplin will not see the plaintext of your notes during the import process. I checked it.
- Don't bother requesting an export of your Apple Notes data from https://privacy.apple.com. They send you only plain text versions after days of waiting for them to process it. Leaving the Apple ecosystem? We're on our own. Let's help each other with our own code!

**Development notes:**

- I found HTML-to-Markdown converters like `pandoc` to be quite incomplete and inaccurate (and `pandoc` requires Homebrew to be installed). So I just put in mostly native AppleScript text substitutions to cover most of what I used in my Apple notes. Ugly code, but reliable result.

- As I wrote this up I eventually discovered a way to export the images directly out of Apple notes via AppleScript, and even other obscure formatting people might use like text colour, and best of all it was a method that would do it in the background so you could continue using the computer - way superior UX. (Basically, it's grabbing the original raw HTML from every note reading the `body` property and hacking the hell out of that HTML to extract and convert base64 image strings into image files and place them into the Joplin-ready `resources` folder.) But I already had finished my AppleScript 'macro' method here and I didn't want to spend more time completely rewriting it. I only had 20 images so I manually dragged them over that way. If someone wants to hack that together and issue an epic PR or fork, be my guest and take over. :)

- To get around the `ARG_MAX` limit for super long Apple Notes, I think script can be changed to create temporary file and process note text that way. It's an idea for anyone who wants to contribute that.

**Feel free to create issues for bugs or feature suggestions, but only if you have a working code change to accompany it. I will test and merge. Thanks!**