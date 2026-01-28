# Blog in a Box - Your Personal Blog, Built by AI

Get your own blog online in 30 minutes. No coding required.

## What This Does

Blog in a Box gives you a live blog website where you can publish your writing. An AI assistant (Claude) builds and manages everything for you. You just write and click "publish."

Your blog will be:
- Live on the internet with its own web address
- Public (anyone can read it)
- Simple and fast - just text, no clutter
- Free to run (using free hosting services)

## What You Need

**Note**: The setup is different for Mac and Windows, but once installed, the "How to Create Your Blog" section below is the same for both.

### For Mac Users

1. Install **Homebrew** (a tool installer for Mac):
   - Open Terminal (find it in Applications > Utilities)
   - Paste this command and press Enter:
     ```
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
     ```
   - Follow the instructions on screen

2. Install **Node.js** (software that runs websites):
   - In Terminal, type: `brew install node`
   - Press Enter and wait for it to finish

3. Install **Claude Code** (the AI that builds your blog):
   - In Terminal, paste this command and press Enter:
     ```
     curl -fsSL https://claude.ai/install.sh | bash
     ```
   - Wait for it to finish, then close and reopen Terminal

### For Windows Users

1. Install **Node.js**:
   - Go to [nodejs.org](https://nodejs.org)
   - Download the "LTS" version (recommended for most users)
   - Run the installer and follow the installation wizard
   - Accept all the default settings

2. Install **Git**:
   - Go to [git-scm.com](https://git-scm.com/download/win)
   - Download and run the installer
   - Accept all the default settings during installation

3. Install **Claude Code**:
   - Open PowerShell (search for "PowerShell" in the Start menu)
   - Paste this command and press Enter:
     ```
     irm https://claude.ai/install.ps1 | iex
     ```
   - Wait for it to finish, then close and reopen PowerShell

### Free Accounts You Need (Mac and Windows)

- **GitHub account**: Go to [github.com](https://github.com) and sign up
- **Vercel account**: Go to [vercel.com](https://vercel.com) and sign up (use "Continue with GitHub" button)
- **Anthropic account** (for Claude): Go to [anthropic.com](https://anthropic.com) and sign up

## How to Create Your Blog

**Before you start**: Make sure you created the three accounts described above (GitHub, Vercel, and Anthropic).

**Note**: These steps work the same on Mac and Windows. On Mac, use Terminal. On Windows, use PowerShell.

1. **Open your command line**:
   - **Mac**: Open Terminal (Applications > Utilities > Terminal)
   - **Windows**: Open PowerShell (search for "PowerShell" in the Start menu)

2. **Create folders and download setup instructions**:
   - Copy all the lines below
   - Paste them into Terminal (Mac) or PowerShell (Windows)
   - Press Enter
   ```
   mkdir -p ~/Projects
   cd ~/Projects
   mkdir my-blog
   cd my-blog
   curl -o PROMPT.md https://raw.githubusercontent.com/lucadellanna/blog-in-a-box/main/PROMPT.md
   ```

3. **Start Claude Code**:
   ```
   claude
   ```
   - You'll be asked to log in - follow the instructions
   - A chat window will open

4. **Tell Claude to build your blog**:
   - Type this message into Claude Code and press Enter:
   ```
   Please read the PROMPT.md file in this folder and follow all instructions in it
   ```

5. **Answer a few questions**:
   - Claude will ask for your blog name, repo name, etc.
   - Just type your answers and press Enter
   - Claude does everything else automatically

6. **Wait 10-20 minutes**:
   - Claude builds your blog
   - Sets up your GitHub repository
   - Deploys it to the internet
   - When done, Claude gives you your blog's web address

## How to Publish New Posts

You have four options for publishing:

### Option 1: Using Claude Code (Terminal)

1. **Write your post** in any text editor (Notes, TextEdit, etc.)

2. **Open Claude Code** in Terminal:
   ```
   cd ~/Projects/my-blog
   claude
   ```

3. **Paste your post** into Claude Code and say "publish this"

4. **Claude shows you a preview link** - click it to see how it looks

5. **Approve it** - if it looks good, tell Claude "yes, publish it"

Your post is now live on your blog.

### Option 2: Using Claude Desktop App

**Note**: This option requires the Claude Desktop app (not the web version) because it needs to access files on your computer.

1. **Download and install** the [Claude Desktop app](https://claude.ai/download) if you haven't already

2. **Open Claude Desktop** and start a new conversation

3. **Tell Claude**:
   ```
   I have a blog created with Blog in a Box at ~/Projects/my-blog
   Please publish this post: [paste your post here]
   ```

4. **Claude will**:
   - Ask for permission to access your Projects folder (approve this)
   - Create the post
   - Show you a preview link
   - Ask for approval before publishing

Note: The first time, you'll need to grant Claude access to your Projects folder. After that, it remembers.

### Option 3: Using Claude iOS App

**Note**: This option uses the Code tab in the Claude iOS app, which can access your GitHub repository.

1. **Download and install** the [Claude iOS app](https://apps.apple.com/app/claude-by-anthropic/id6473753684) if you haven't already

2. **Open the Claude app** and tap the **Code** tab at the bottom

3. **Connect to your blog's GitHub repository**:
   - Tap "Connect to GitHub" (first time only)
   - Sign in to GitHub and grant access
   - Select your blog repository

4. **Write or paste your post**, then tell Claude:
   ```
   Please publish this post: [your post text here]
   ```

5. **Claude will**:
   - Create the post in your repository
   - Show you a preview link
   - Ask for approval before publishing

This is great for publishing while on the go, without needing your Mac.

### Option 4: Publish a Handwritten Post

**Note**: Use the Claude Desktop app or iOS Code tab for this - the web version can't access your blog files.

1. **Take a clear photo** of your handwritten post with your phone

2. **Using Claude Desktop**:
   - Open the Claude Desktop app
   - Upload the photo (click the paperclip icon or drag the image)
   - Tell Claude: "I have a blog at ~/Projects/my-blog. Please transcribe this handwritten post and publish it"

   **OR using Claude iOS Code tab**:
   - Open the Claude iOS app
   - Tap the Code tab
   - Make sure it's connected to your blog's GitHub repository
   - Upload the photo
   - Tell Claude: "Please transcribe this handwritten post and publish it"

3. **Claude will**:
   - Read your handwriting
   - Convert it to text
   - Format it properly
   - Show you a preview link
   - Ask for approval before publishing

This works great for journal entries, notes written on paper, or posts drafted while away from your computer.

## What's Included

- A home page that lists all your posts
- An "About" page to introduce yourself
- Clean, readable design
- Works on phones and computers

## What's NOT Included

This is intentionally simple. No photos, no comments, no analytics, no ads. Just you and your writing.

But you can ask Claude to add them later if you need them!

## Need Help?

If something goes wrong, copy the error message and paste it back to Claude Code. Claude will fix it.

## Disclaimer

This is provided as-is. The creator is not responsible for any issues, costs, or problems that may arise from using this tool. Use at your own risk.

---

For any questions, contact me at [Luca-Dellanna.com/contact](https://Luca-Dellanna.com/contact)
