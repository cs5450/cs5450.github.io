---
layout: default
title: Labs
nav: labs
---

<a title="Git Official Website" href="http://git-scm.com/">Git</a> is a distributed source code version control system. When you place your code under version control, you record the changes you make to your files over time and you can recall the history of each of your file changes at will. We will be using git extensively this semester in homework assignments.

<a title="GitHub" href="https://github.com/">GitHub</a> is a development ecosystem based around git. In this course, we will be using GitHub to host our git repositories and we will take advantage of other GitHub features such as the issue tracker and wiki.
<h3 id="toc_1">Disclaimer</h3>
The instructions and discussion in this lab are for the git <strong>command line</strong> interface. GitHub and other software vendors have GUI-based applications to interact with repository. These tools are not will not be supported in this class.
<h3 id="toc_2">1. Install Course VM</h3>
Download and Install the Course Virtual Machine. <a title="Installing The Course VM" href="http://bits.usc.edu/cs104_su15/installing-course-vm.html">Instructions are available on this page</a>.
<h3>2. Create a Github account</h3>
We will be using git extensively this semester in labs and in programming assignments. Github is a development ecosystem based around git. In CS 104, we will be using Github to host our git repositories and we will take advantage of other GitHub features such as the <a href="https://github.com/features/projects/issues">issue tracker</a> and <a href="https://github.com/features/projects/wikis">wiki</a>.

If you already have a Github account and you wish to use it for this course, you can skip to next section.

We start by visiting Github's sign-up page. You are free to choose your username and email. It does not necessarily need to match your USCNet account. Your email, however, needs the email you used in your git configuration.
<blockquote><a href="https://github.com/signup/free">https://github.com/signup/free</a></blockquote>
You will be sent an email to verify your email address. Do that before proceeding.
<h3 id="toc_7">2. Git Configuration – SSH Keys</h3>
One of the main features of using a distributed version control system such as git, is having a complete backup of your code and its history. Git uses the <a href="http://en.wikipedia.org/wiki/Secure_Shell">Secure Shell</a> protocol (SSH) when contacting remote servers. To facilitate this communication, you need to generate a pair of encryption keys: one public and the other private.

In this step, we will generate the set of keys required to use SSH. This will be done manually through the command line. To start the configuration you would need to launch the Terminal.

Then, type the following command replacing Tommy Trojan's email with your own:
<pre>ssh-keygen -t rsa -b 2048 -C "ttrojan@usc.edu"</pre>
When prompted for a file location, use the default location <em>(press Enter)</em>:
<div class="highlight">
<pre># Creates a new ssh key, using the provided email as a label
Generating public/private rsa key pair.
Enter file in which to save the key (/home/student/.ssh/id_rsa):
</pre>
</div>
After that, you will be prompted for a passphrase to secure your private key. It is a good idea to secure your key with a passphrase, though it is optional.

Upon success, you should receive a confirmation such as:
<div class="highlight">
<pre>Your identification has been saved in /home/student/.ssh/id_rsa.
Your public key has been saved in /home/student/.ssh/id_rsa.pub.
The key fingerprint is:
01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db ttrojan@usc.edu
</pre>
</div>
<h3 id="toc_8">3. Git Configuration – Developer Profile</h3>
The next step is to configure your git <em>profile</em> in your development environment. This is <strong>important</strong> because your profile information is used to annotate your <em>contributions</em> to a repository/project. Although this may not seem like a big deal when you are the only one committing to a repository, it is of the utmost importance once you work on a group project because this information is the basis used to calculate <a href="https://github.com/jquery/jquery/contributors">your contribution</a> to a project.

You only need to do this configuration once and we will be setting-up the following information:
<ul>
	<li>Name, e.g. <code>Tommy Trojan</code></li>
	<li>Email, e.g. <code>ttrojan@usc.edu</code></li>
	<li>Editor for commit messages, e.g. <code>vi</code>, <code>emacs</code>, <code>notepad</code></li>
	<li>Git message colors (make it pretty-er)</li>
	<li>Properly handle <a href="http://en.wikipedia.org/wiki/Newline#Common_problems">new lines</a></li>
</ul>
Configuration is performed manually through the command line. To start the configuration you would need to launch Terminal
<h5 id="toc_9">Your Personal Information</h5>
Please use your actual first and last name when configuring your git <code>user.name</code>. For your email, you should use the email address you want to appear in the <a href="https://github.com/jquery/jquery/commits">git commit log</a>. You should not feel obligated to use your <code>usc.edu</code> account here unless you want to. Configure your information as follows:
<div class="highlight">
<pre><span class="c"># Set your name and email</span>
git config --global user.name <span class="s2">"Tommy Trojan"</span>
git config --global user.email <span class="s2">"ttrojan@usc.edu"</span>
</pre>
</div>
<strong>Note:</strong> One of the cool features of the GitHub UI is linking your contributions to your <a href="https://github.com/blog/1360-introducing-contributions">GitHub profile</a>. For that to work, you <em><strong>MUST</strong></em> register and verify your git <code>user.email</code> in the <a href="https://github.com/settings/emails">GitHub Email Settings</a>. (Hint: you can have multiple emails registered with GitHub and you could control which ones are public and/or private).
<h5 id="toc_10">Git CLI Interface</h5>
By default, git does not color its output. Pretty printing git messages makes it easy read the output and take proper actions. You can enable colors for interactive use of git by:
<div class="highlight">
<pre><span class="c"># Let's get pretty colored output!</span>
git config --global color.ui auto
</pre>
</div>
<h5 id="toc_11">Git Message Text Editor</h5>
When committing code in git, the system requires a commit message. If a message is not provided via the commandline, git will launch the Operating System's default text editor prompting you for a message. You can customize this action using the following configuration directive:
<div class="highlight">
<pre><span class="c"># Pick your editor of choice to edit commit messages (not code)</span>
<span class="c"># Note that you should pick an editor that is installed on your machine</span>
git config --global core.editor vim
</pre>
</div>
Here, git will automatically launch <code>vim</code>. You may want to change that to your editor of choice. Here are some examples (you only need one):
<div class="highlight">
<pre><span class="c"># emacs</span>
git config --global core.editor emacs

<span class="c"># Sublime Text</span>
git config --global core.editor "subl -n -w"
</pre>
</div>
<h5 id="toc_12">Let Git Handle New Lines</h5>
Operating Systems implement <a href="http://en.wikipedia.org/wiki/Newline#Common_problems">new lines</a> differently. Here you will configure git to automatically normalize the line feed (<code>LF</code>) to properly match the platform. Please use the proper configuration command for your OS:
<div class="highlight">
<pre><span class="c"># LF Normalization (Linux &amp; MacOS)</span>
git config --global core.autocrlf input</pre>
</div>
<h5 id="toc_13">Check Your Work</h5>
<div class="highlight">
<pre><span class="c"># List local git configuration options</span>
git config --list
</pre>
</div>
<h3 id="toc_14">4. Update your Github Profile</h3>
You need to update your profile to include your name and SSH public key. There are also some optional settings you can change such as your avatar (profile picture) and email notifications.

In your <a href="https://github.com/settings/profile">Profile Settings</a>:
<ul>
	<li>Put your <strong>real name</strong> in the name field. This is to ensure the TAs &amp; Sherpas know who you are regardless of your username.</li>
	<li>[Optional] Change your <a href="http://gravatar.com/">avatar</a></li>
</ul>
In your <a href="https://github.com/settings/ssh">SSH Key Settings</a>:
<ul>
	<li>Click on Add SSH Key</li>
	<li>Provide a name for the key, such as "CS104 VM Key"</li>
	<li>Copy the contents of your id_rsa.pub file and paste them into the key field
<div class="highlight">
<pre>cat ~/.ssh/id_rsa.pub
</pre>
</div></li>
	<li>Click Add Key</li>
</ul>
<strong>[Optional]</strong> You can add more emails to your GitHub account using the <a href="https://github.com/settings/emails">Email Settings</a> page.

<strong>[Optional]</strong> Set your notification preferences on the <a href="https://github.com/settings/notifications">Notification Settings</a> page.
<h3 id="toc_17">5. The Hello World Repository</h3>
Now, let's create our own repositry and using git to keep track of our code. Start by creating a public repository called HelloWorld using GitHub:
<blockquote><a href="https://github.com/new">https://github.com/new</a></blockquote>
Use the following options:
<ul>
	<li>Repositroy Name: <strong>HelloWorld</strong></li>
	<li>Description: <strong>CSCI 104 - Lab01 Sample Repository</strong></li>
	<li>This should be a <strong>public</strong> repository</li>
	<li><strong>[x]</strong> Initialize this repository with a README</li>
	<li>Add a <code>.gitignore</code> file for <strong>C++</strong>.</li>
</ul>
<img src="http://bits.usc.edu/cs104_su15/labs/img/github_create-repo.png" alt="Create a New Repository in GitHub" />
<h4 id="toc_18">Clone Your Repository</h4>
At this stage, we created a place to host our code called a code <em>repository</em>. We now need to make a local version of the repository to work with it. The is called <em>cloning</em> the repository. To do that, you need to get the cloning URL by visiting the page of the repository you just created in GitHub and look for the <strong>SSH clone URL</strong>.

<img src="http://bits.usc.edu/cs104_su15/labs/img/github_clone-ssh.png" alt="Clone SSH" />

Using that URL, you can clone the repo using the following commands in the Terminal or GitBash if you were on Windows.
<div class="highlight">
<pre><span class="c"># Change directory to your home directory</span>
<span class="nb">cd</span>
<span class="c"># Clone the repository</span>
git clone git@github.com:ttrojan/HelloWorld.git
<span class="c"># Start working on the repository</span>
<span class="nb">cd </span>HelloWorld
</pre>
</div>
<h4 id="toc_19">HelloWorld.cpp</h4>
Use your favorite editor to write a HelloWorld program in a file called <code>HelloWorld.cpp</code>:
<div class="highlight">
<pre><span class="cp"># include &lt;iostream&gt;</span>

<span class="kt">int</span> <span class="nf">main</span><span class="p">()</span> <span class="p">{</span>
    <span class="n">std</span><span class="o">::</span><span class="n">cout</span> <span class="o">&lt;&lt;</span> <span class="s">"HelloWorld!"</span> <span class="o">&lt;&lt;</span> <span class="n">std</span><span class="o">::</span><span class="n">endl</span><span class="p">;</span>
<span class="p">}</span>
</pre>
</div>
Since we have made progress in our code, it is a good idea to <em>commit</em> that code to the repository and make part of our code history. To see what files changed, you run the following command:
<div class="highlight">
<pre>git status
</pre>
</div>
You will get something like:
<div class="highlight">
<pre>On branch master
Your branch is up-to-date with 'origin/master'.

Untracked files:
  (use "git add &lt;file&gt;..." to include in what will be committed)

        HelloWorld.cpp

nothing added to commit but untracked files present (use "git add" to track)
</pre>
</div>
In the above status message, git is telling you that you have one file that is not currently tracked by your repository and that file is called<code>HelloWorld.cpp</code>. If you want to add the file to your repository, you need to use the <code>git add &lt;file&gt;</code> command. Here is an explanation of the some of the terms:
<ul>
	<li><strong>branch master</strong>: you are currently working on the main thread of developement that is called by default: <em>master</em>.</li>
	<li><strong>origin/master</strong>: in this context, origin/master refers to the <em>master</em> on GitHub.</li>
	<li><strong>Your branch is up-to-date with 'origin/master'</strong>: your local repository is sync-ed with the GitHub server. ( <em>Hint</em>: this is since the last time you received and update from the server).</li>
	<li><strong>Untracked files</strong>: files that are not part of the repository and were never added to the repository before.</li>
</ul>
<h4 id="toc_20">Commiting Changes</h4>
To <em>track</em> <code>HelloWorld.cpp</code>, we use the following command:
<div class="highlight">
<pre><span class="c"># Add HelloWorld.cpp</span>
git add HelloWorld.cpp
</pre>
</div>
Now, let's check the status of our repository:
<div class="highlight">
<pre>git status
</pre>
</div>
You notice we get:
<div class="highlight">
<pre>On branch master
Your branch is up-to-date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD &lt;file&gt;..." to unstage)

        new file:   HelloWorld.cpp
</pre>
</div>
This tells us that we just added a new file called <code>HelloWorld.cpp</code> and we are ready to commit. Technically, we moved this new file from being<em>untracked</em> to be <em>staged</em> for commit. You can continue working on other files and you can <em>stage</em> them too. Once you feel that you are ready to committed, i.e. be part of the repository history, you use the following command:
<div class="highlight">
<pre><span class="c"># Commit with a message</span>
git commit -m <span class="s2">"My first HelloWorld using git"</span>
</pre>
</div>
This will commit <code>HelloWorld.cpp</code> and make it part of repository history. Each commit <em>must</em> have a message associated with it. You can add the message as part of the commit command using the <code>-m</code> option (see code above) or git will prompt you for a message using the editor you configured in section 3.

To check the commit history for the repository, you use the <code>log</code> command:
<div class="highlight">
<pre><span class="c"># See the commit history of the repository</span>
git log
</pre>
</div>
Which will give us the following message
<div class="highlight">
<pre>commit df7cd3feda8a856de9cb2dc4bc132f15f7842bb1
Author: Tommy Trojan &lt;ttrojan@usc.edu&gt;
Date:   Tue Jan 14 17:42:52 2014 -0800

    My first HelloWorld using git

commit 6d9fe80012ff9bf5b43120a87dc61bf196fec313
Author: Tommy Trojan &lt;ttrojan@usc.edu&gt;
Date:   Tue Jan 14 15:57:08 2014 -0800

    Initial commit
</pre>
</div>
The commit history of this repository shows that we have two commits in reverse chronological order. For each commit, you see the commit id (which is a SHA1 checksum), the author, the time the commit was made and the commit message.
<h4 id="toc_21">Pushing to the Server</h4>
Now that we committed successfully, let's check the status of our repository
<div class="highlight">
<pre><span class="c"># Check status of repository</span>
git status
</pre>
</div>
You will notice something different about this message:
<div class="highlight">
<pre>On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working directory clean
</pre>
</div>
It says that we are: <strong>ahead of 'origin/master' by 1 commit</strong>. If you go back to your repository, you will find that it only has one commit as opposed to the two you have locally. Since git is distributed, it allows you to work "offline" by committing locally and only pushing to the server once you feel ready to do so.

<strong>Hint</strong>: commit often and commit early. Try to push to the server as soon as you have a good portion of your code complete so you have a backup of your code. Never leave your coding session before pushing to the server.

To <em>push</em> your changes to the server, you use the following command
<div class="highlight">
<pre><span class="c"># Push changes to remote</span>
git push
</pre>
</div>
Now, check the commits section of your code repository by clicking on the <em>commits</em> links in your repository's page

<img src="http://bits.usc.edu/cs104_su15/labs/img/github_link-commits.png" alt="GitHub Commits Page link" />
<h4 id="toc_22">Keeping Your Repo Up to Date</h4>
Since git uses the distributed model, you can have multiple copies of the repository on multiple machines. This makes it important to make sure your local version is up to date. Here is a practical example of what could happen.
<ol>
	<li>Go to your repository on GitHub</li>
	<li>Click on the <code>HelloWorld.cpp</code> file</li>
	<li>Click on <strong>Edit</strong> to edit the file online</li>
	<li>Let's update <code>HelloWorld.cpp</code>:
<ul>
	<li>Change <code>HelloWorld!</code> to <code>FightOn!</code></li>
	<li>In <em>commit summary</em> type: <code>School pride</code></li>
</ul>
</li>
</ol>
Visit the commits page in your repository and note how many commits you have. Now, go to your local repository and check the status of your repository:
<div class="highlight">
<pre><span class="c"># Check status of repository</span>
git status
</pre>
</div>
You should get
<div class="highlight">
<pre>On branch master
nothing to commit, working directory clean
</pre>
</div>
If you check the commit history, how many commits will you see:
<div class="highlight">
<pre><span class="c"># Produce commit history with one line per commit</span>
git log --pretty<span class="o">=</span>oneline
</pre>
</div>
To bring your local repository up to date, you need to <em>pull</em> from the server:
<div class="highlight">
<pre><span class="c"># Get the latest updates</span>
git pull
</pre>
</div>
You will get a response that looks like:
<div class="highlight">
<pre>From git@github.com:ttrojan/HelloWorld.git
 * branch            master     -&gt; FETCH_HEAD
Updating df7cd3f..1ffe7e8
Fast-forward
 HelloWorld.cpp | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
</pre>
</div>
Here, git is telling you: + It is updating from <a href="mailto:git@github.com">git@github.com</a>:ttrojan/HelloWorld.git + It updated <em>master</em> + The update was from commit <code>df7cd3f</code> to commit <code>1ffe7e8</code> + Only one file was updated: - File name is: <code>HelloWorld.cpp</code> - There was one line <em>inserted</em> - There was one line <em>deleted</em>

If you want to see in detail what the changes in the <em>last</em> commit were, you issue this command:
<div class="highlight">
<pre>git log -p -1
</pre>
</div>
To see
<div class="highlight">
<pre>commit 1ffe7e8b5e776395126fb6e06fc72f5af12ab063
Author: Tommy Trojan &lt;ttrojan@usc.edu&gt;
Date:   Tue Jan 14 18:53:08 2014 -0800

    School pride

<span class="gh">diff --git a/HelloWorld.cpp b/HelloWorld.cpp</span>
<span class="gh">index 55818b8..b9a284d 100644</span>
<span class="gd">--- a/HelloWorld.cpp</span>
<span class="gi">+++ b/HelloWorld.cpp</span>
<span class="gu">@@ -1,5 +1,5 @@</span>
 #include &lt;iostream&gt;

 int main() {
<span class="gd">-       std::cout &lt;&lt; "HelloWorld!" &lt;&lt; std::endl;</span>
<span class="gi">+       std::cout &lt;&lt; "FightOn!" &lt;&lt; std::endl;</span>
 }
</pre>
</div>
<h4 id="toc_23">Ignoring Files</h4>
Code repositories are intended for just that, code. When working on a programming project, you may get a number of different files such as: object files, executables, log files and sometimes compiled header or library files. It is <strong>important</strong> to keep your repository clean of such unecessery files.

git uses a file named <code>.gitignore</code> to list all files or file extensions that git should not track or report when you do a <code>git status</code>. Your repository now has a list that was auto generated by GitHub. You can see that list by typing the following command from within your repository:
<div class="highlight">
<pre><span class="c"># List the contents of .gitignore</span>
cat .gitignore
</pre>
</div>
Let's compile our <code>HelloWorld.cpp</code> to get the executable <code>helloworld</code>:
<div class="highlight">
<pre><span class="c"># Compile HelloWorld.cpp</span>
g++ HelloWorld.cpp -o helloworld
<span class="c"># run the executable</span>
./helloworld
</pre>
</div>
Now, if we do a <code>git status</code>, we will get the following output:
<div class="highlight">
<pre>On branch master
Untracked files:
  (use "git add &lt;file&gt;..." to include in what will be committed)

        helloworld

nothing added to commit but untracked files present (use "git add" to track)
</pre>
</div>
Since <code>helloworld</code> is an executable, we don't want to add it to the repository and we definitly don't want <code>git status</code> to keep bugging us about it. So, append <code>.gitignore</code> to have <code>helloworld</code>.

Now, do a <code>git status</code> and examine the output:
<div class="highlight">
<pre>On branch master
Changes not staged for commit:
  (use "git add &lt;file&gt;..." to update what will be committed)
  (use "git checkout -- &lt;file&gt;..." to discard changes in working directory)

        modified:   .gitignore

no changes added to commit (use "git add" and/or "git commit -a")
</pre>
</div>
As you can see, we are no longer prompted for <code>helloworld</code>, however, we now need to commit our changes to .gitignore.
<div class="highlight">
<pre><span class="c"># Add .gitignore after modifying it</span>
git add .gitignore
<span class="c"># Commit the changes</span>
git commit -m <span class="s2">"added helloworld to the list of ignored files"</span>
<span class="c"># Pushing the changes to the server</span>
git push
</pre>
</div>
<h4 id="toc_24">Let's Git Going</h4>
To summarize, we learned the following git commands:
<div class="highlight">
<pre><span class="c"># Clone a repository</span>
git clone git@github.com:ttrojan/HelloWorld.git
<span class="c"># Add an untracked or modified file</span>
git add &lt;file1&gt; &lt;file2&gt; &lt;...&gt;
<span class="c"># Commit files that are staged for commit</span>
git commit -m <span class="s2">"non-optional commit message"</span>
<span class="c"># Push commits to the server</span>
git push

<span class="c"># Check the status of the repo</span>
git status

<span class="c"># Let's make sure our repository is up to date</span>
git pull

<span class="c"># See the commit history</span>
git log
<span class="c"># See the commit history by printing a summary of each commit in a line</span>
git log --pretty<span class="o">=</span>oneline
<span class="c"># See the log of the last commit (-1) showing diffs of all the files (-p)</span>
git log -p -1
</pre>
</div>
<h3 id="toc_25">6. Give Us Your GitHub Account</h3>
In this course, we will give your own private repository for your homework and projects. To do that, we need to know your GitHub Account.

Please fill the <a href="https://docs.google.com/a/usc.edu/forms/d/1ZESonA_dklq81fH3uyB4eiRtNJ87Z_89j06w_ZyziTU/viewform">CSCI 104 – GitHub Account Registration</a>.

Note: You need your <a href="http://google.usc.edu/">Google Apps</a> for USC account enabled to use the survey.
<h3 id="toc_26">Git Resources</h3>
For more on git and how to use it, see the <a href="http://bits.usc.edu/cs104_su15/git-resources.html">Git Resources</a> page.