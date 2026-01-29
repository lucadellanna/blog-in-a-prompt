# Blog in a Prompt - Your Personal Blog, Built by AI

Get your own blog online in 30 minutes. No coding required.

## What This Does

Blog in a Prompt gives you a live blog website where you can publish your writing. An AI assistant (Claude) builds and manages everything for you. You just write and click "publish."

Your blog will be:
- Live on the internet with its own web address
- Public (anyone can read it)
- Simple and fast - just text, no clutter
- Free to run (using free hosting services)*

*Claude Code may require a paid plan (check claude.ai for current pricing). GitHub and Vercel hosting are free on their free tiers.

## What You Need

### Step 1: Create Accounts (Everyone - Do This First)

**Create these three accounts before installing any software:**

- **GitHub account**: Go to [github.com](https://github.com) and sign up. This is where your blog's files will be stored (in what's called a "repository" - like a folder in the cloud).
- **Vercel account**: Go to [vercel.com](https://vercel.com) and sign up (use "Continue with GitHub" button)
- **Claude account**: Go to [claude.ai](https://claude.ai) and sign up. You may need a paid plan to use Claude Code (check claude.ai for current pricing).

You'll use these accounts during the blog setup process.

### Step 2: Install Software

**Note**: The setup is different for Mac and Windows, but once installed, the "How to Create Your Blog" section below is the same for both.

#### For Mac Users

1. Install **Homebrew** (a tool installer for Mac):
   - Open Terminal (Applications folder > Utilities folder > Terminal)
   - Paste this command and press Enter:
     ```
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
     ```
   - Follow the instructions on screen (this may take a few minutes)
   - When it's done, you'll see "Installation successful" and your prompt will return (the line where you can type again). You may also be prompted to install Apple's Command Line Tools (includes Git), which Claude uses to save your blog's files.

2. Install **Node.js** (software that runs websites):
   - In Terminal, paste this command and press Enter:
     ```
     brew install node
     ```
   - You'll know it's done when you see your prompt again (the line where you can type)

3. Install **Claude Code** (the AI that builds your blog):
   - In Terminal, paste this command and press Enter:
     ```
     curl -fsSL https://claude.ai/install.sh | bash
     ```
   - You'll see a message confirming Claude Code was installed
   - Close and reopen Terminal before continuing

#### For Windows Users

1. Install **Node.js**:
   - Go to [nodejs.org](https://nodejs.org)
   - Download the "LTS" version (LTS stands for "Long Term Support" - it's the most stable version, recommended for most users)
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
   - You'll see a message confirming Claude Code was installed
   - Close and reopen PowerShell before continuing

## How to Create Your Blog

**Note**: These steps work the same on Mac and Windows. On Mac, use Terminal. On Windows, use PowerShell.

1. **Open Terminal (Mac) or PowerShell (Windows)**:
   These apps are sometimes called the "command line" or "CLI" - they let you type commands to control your computer.
   - **Mac**: Open Terminal (Applications > Utilities > Terminal)
   - **Windows**: Open PowerShell (search for "PowerShell" in the Start menu)

2. **Create folders and download setup instructions**:
   These commands create a folder for your blog and download the instructions that Claude will follow to build it.

   Note: `~/` (Mac) and `$HOME/` (Windows) mean your home folder - like /Users/YourName on Mac or C:\Users\YourName on Windows.

   The commands are different for Mac and Windows. Copy the entire block for your operating system:

   **On Mac (in Terminal):**
   ```bash
   mkdir -p ~/Projects/my-blog
   cd ~/Projects/my-blog
   curl -o PROMPT.md https://raw.githubusercontent.com/lucadellanna/blog-in-a-prompt/main/PROMPT.md
   ```

   **On Windows (in PowerShell):**
   ```powershell
   New-Item -Path "$HOME/Projects/my-blog" -ItemType Directory -Force
   cd "$HOME/Projects/my-blog"
   curl.exe -o PROMPT.md https://raw.githubusercontent.com/lucadellanna/blog-in-a-prompt/main/PROMPT.md
   ```

3. **Start Claude Code**:
   ```
   claude
   ```
   - You'll be asked to log in with your Anthropic account - follow the instructions
   - A chat window will open in your Terminal/PowerShell

4. **Tell Claude to build your blog**:
   - Type this message into Claude Code and press Enter:
   ```
   Please read the PROMPT.md file in this folder and follow all instructions in it
   ```

   **What to expect**: Over the next 10-15 minutes, Claude will ask you a few questions, then work automatically. You'll see a lot of text scrolling by - this is normal! Claude is building your blog.

5. **Answer a few questions**:
   - Claude will ask for your blog name, your name for the About page, etc.
   - Just type your answers and press Enter
   - Claude does everything else automatically

6. **Wait for Claude to finish** (10-15 minutes):
   - Claude builds your blog
   - Sets up your GitHub repository
   - Deploys it to the internet
   - When done, Claude gives you your blog's web address

   Once complete, your blog is live! You can close Terminal/PowerShell. The blog stays online - you don't need to keep anything running.

   **If you accidentally close Terminal/PowerShell during setup**: Don't worry - just reopen it, navigate to your blog folder (`cd ~/Projects/my-blog`), type `claude`, and tell Claude to continue where it left off.

## How to Publish New Posts

You have four options for publishing. **Recommended for beginners: Option 1** - it uses the same Terminal/PowerShell you used to create the blog. Options 2-4 are alternatives for specific situations.

### Option 1: Using Claude Code (Terminal) - Recommended

1. **Write your post** in any text editor (Notes, TextEdit, etc.)

2. **Open Claude Code** in Terminal (Mac) or PowerShell (Windows):
   - First, paste this line and press Enter:
   ```
   cd ~/Projects/my-blog
   ```
   - Then, paste this line and press Enter:
   ```
   claude
   ```

3. **Paste your post** into Claude Code and type exactly: "publish this"

4. **Claude shows you a preview link** - click it to see how it looks

5. **Approve it** - if it looks good, tell Claude "yes, publish it"

Your post is now live on your blog.

### Option 2: Using Claude Desktop App

**Note**: This option requires the Claude Desktop app (not the web version) because it needs to access files on your computer.

1. **Download and install** the [Claude Desktop app](https://claude.ai/download) if you haven't already

2. **Open Claude Desktop** and start a new conversation

3. **Tell Claude** (replace the bracketed text with your actual post):
   ```
   I have a blog created with Blog in a Prompt at ~/Projects/my-blog
   Please publish this post: [paste your post here]
   ```
   Example: "I have a blog created with Blog in a Prompt at ~/Projects/my-blog. Please publish this post: Today I learned about..."

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
   - Select your blog repository (it will have the name you chose when creating your blog)

4. **Write or paste your post**, then tell Claude (replace the bracketed text with your actual post):
   ```
   Please publish this post: [your post text here]
   ```
   Example: "Please publish this post: Today I learned about..."

5. **Claude will**:
   - Create the post in your repository
   - Show you a preview link
   - Ask for approval before publishing

This is great for publishing while on the go, without needing your computer.

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

**Common questions:**
- **Where's my blog URL?** Claude gave it to you when setup finished. If you lost it, you can find it by opening the `.vercel-url` file inside your `my-blog` project folder. You can also ask Claude "what's my blog URL?"
- **Can I change my blog name later?** Yes! Just ask Claude to update it.
- **How do I see all my posts?** Go to your blog's home page - it lists all posts.

## Security & Privacy

**Remember**: Your blog is PUBLIC. Anyone on the internet can read it.

**Never publish**:
- Passwords or API keys
- Private personal information (address, phone number, social security numbers, etc.)
- Confidential business information
- Anything you wouldn't want the whole world to see

Always review the preview before publishing to make sure you're comfortable with the content going public.

## Disclaimer

This is provided as-is. The creator is not responsible for any issues, costs, or problems that may arise from using this tool. Use at your own risk.

---

For any questions, [contact me](https://luca-dellanna.com/contact/).

## About the Author

I'm Luca Dellanna ([luca-dellanna.com](https://luca-dellanna.com)), an independent management consultant helping businesses nail change initiatives (including AI) and the author of several books ([view my books](https://luca-dellanna.com/books)).
