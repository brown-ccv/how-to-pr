# Git

Source: [Wikipedia](https://en.wikipedia.org/wiki/Git)

Git (/ɡɪt/)[8] is free and open source software for distributed version control: tracking changes in any set of files, usually used for coordinating work among programmers collaboratively developing source code during software development. Its goals include speed, data integrity, and support for distributed, non-linear workflows (thousands of parallel branches running on different systems).[9][10][11]

Git was originally authored by Linus Torvalds in 2005 for development of the Linux kernel, with other kernel developers contributing to its initial development.[12] Since 2005, Junio Hamano has been the core maintainer. As with most other distributed version control systems, and unlike most client–server systems, every Git directory on every computer is a full-fledged repository with complete history and full version-tracking abilities, independent of network access or a central server.[13] Git is free and open-source software distributed under the GPL-2.0-only license.

## History
Git development began in April 2005, after many developers of the Linux kernel gave up access to BitKeeper, a proprietary source-control management (SCM) system that they had been using to maintain the project since 2002.[14][15] The copyright holder of BitKeeper, Larry McVoy, had withdrawn free use of the product after claiming that Andrew Tridgell had created SourcePuller by reverse engineering the BitKeeper protocols.[16] The same incident also spurred the creation of another version-control system, Mercurial.

Linus Torvalds wanted a distributed system that he could use like BitKeeper, but none of the available free systems met his needs. Torvalds cited an example of a source-control management system needing 30 seconds to apply a patch and update all associated metadata, and noted that this would not scale to the needs of Linux kernel development, where synchronizing with fellow maintainers could require 250 such actions at once. For his design criterion, he specified that patching should take no more than three seconds, and added three more goals:[9]

Take Concurrent Versions System (CVS) as an example of what not to do; if in doubt, make the exact opposite decision.[11]
Support a distributed, BitKeeper-like workflow.[11]
Include very strong safeguards against corruption, either accidental or malicious.[10]
These criteria eliminated every version-control system in use at the time, so immediately after the 2.6.12-rc2 Linux kernel development release, Torvalds set out to write his own.[11]

The development of Git began on 3 April 2005.[17] Torvalds announced the project on 6 April and became self-hosting the next day.[17][18] The first merge of multiple branches took place on 18 April.[19] Torvalds achieved his performance goals; on 29 April, the nascent Git was benchmarked recording patches to the Linux kernel tree at the rate of 6.7 patches per second.[20] On 16 June, Git managed the kernel 2.6.12 release.[21]

Torvalds turned over maintenance on 26 July 2005 to Junio Hamano, a major contributor to the project.[22] Hamano was responsible for the 1.0 release on 21 December 2005.[23]

### Naming
Torvalds sarcastically quipped about the name git (which means "unpleasant person" in British English slang): "I'm an egotistical bastard, and I name all my projects after myself. First 'Linux', now 'git'."[24][25] The man page describes Git as "the stupid content tracker".[26] The read-me file of the source code elaborates further:[27]

"git" can mean anything, depending on your mood.

Random three-letter combination that is pronounceable, and not actually used by any common UNIX command. The fact that it is a mispronunciation of "get" may or may not be relevant.
Stupid. Contemptible and despicable. Simple. Take your pick from the dictionary of slang.
"Global information tracker": you're in a good mood, and it actually works for you. Angels sing, and a light suddenly fills the room.
"Goddamn idiotic truckload of sh*t": when it breaks.
The source code for Git refers to the program as, "the information manager from hell."

Design
Git's design was inspired by BitKeeper and Monotone.[38][39] Git was originally designed as a low-level version-control system engine, on top of which others could write front ends, such as Cogito or StGIT.[39] The core Git project has since become a complete version-control system that is usable directly.[40] While strongly influenced by BitKeeper, Torvalds deliberately avoided conventional approaches, leading to a unique design.[41]

### Characteristics

Git's design is a synthesis of Torvalds's experience with Linux in maintaining a large distributed development project, along with his intimate knowledge of file-system performance gained from the same project and the urgent need to produce a working system in short order. These influences led to the following implementation choices:[42]

#### Strong support for non-linear development
Git supports rapid branching and merging, and includes specific tools for visualizing and navigating a non-linear development history. In Git, a core assumption is that a change will be merged more often than it is written, as it is passed around to various reviewers. In Git, branches are very lightweight: a branch is only a reference to one commit. With its parental commits, the full branch structure can be constructed.[improper synthesis?]
#### Distributed development
Like Darcs, BitKeeper, Mercurial, Bazaar, and Monotone, Git gives each developer a local copy of the full development history, and changes are copied from one such repository to another. These changes are imported as added development branches and can be merged in the same way as a locally developed branch.[43]
#### Compatibility with existing systems and protocols
Repositories can be published via Hypertext Transfer Protocol (HTTP), File Transfer Protocol (FTP), or a Git protocol over either a plain socket or Secure Shell (ssh). Git also has a CVS server emulation, which enables the use of existing CVS clients and IDE plugins to access Git repositories. Subversion repositories can be used directly with git-svn.[44]
Efficient handling of large projects
Torvalds has described Git as being very fast and scalable,[45] and performance tests done by Mozilla[46] showed that it was an order of magnitude faster diffing large repositories than Mercurial and GNU Bazaar; fetching version history from a locally stored repository can be one hundred times faster than fetching it from the remote server.[47]

#### Cryptographic authentication of history

The Git history is stored in such a way that the ID of a particular version (a commit in Git terms) depends upon the complete development history leading up to that commit. Once it is published, it is not possible to change the old versions without it being noticed. The structure is similar to a Merkle tree, but with added data at the nodes and leaves.[48] (Mercurial and Monotone also have this property.)
#### Toolkit-based design
Git was designed as a set of programs written in C and several shell scripts that provide wrappers around those programs.[49] Although most of those scripts have since been rewritten in C for speed and portability, the design remains, and it is easy to chain the components together.[50]
Pluggable merge strategies
As part of its toolkit design, Git has a well-defined model of an incomplete merge, and it has multiple algorithms for completing it, culminating in telling the user that it is unable to complete the merge automatically and that manual editing is needed.[51]
Garbage accumulates until collected
Aborting operations or backing out changes will leave useless dangling objects in the database. These are generally a small fraction of the continuously growing history of wanted objects. Git will automatically perform garbage collection when enough loose objects have been created in the repository. Garbage collection can be called explicitly using git gc.[52]
Periodic explicit object packing
Git stores each newly created object as a separate file. Although individually compressed, this takes up a great deal of space and is inefficient. This is solved by the use of packs that store a large number of objects delta-compressed among themselves in one file (or network byte stream) called a packfile. Packs are compressed using the heuristic that files with the same name are probably similar, without depending on this for correctness. A corresponding index file is created for each packfile, telling the offset of each object in the packfile. Newly created objects (with newly added history) are still stored as single objects, and periodic repacking is needed to maintain space efficiency. The process of packing the repository can be very computationally costly. By allowing objects to exist in the repository in a loose but quickly generated format, Git allows the costly pack operation to be deferred until later, when time matters less, e.g., the end of a workday. Git does periodic repacking automatically, but manual repacking is also possible with the git gc command. For data integrity, both the packfile and its index have an SHA-1 checksum inside, and the file name of the packfile also contains an SHA-1 checksum. To check the integrity of a repository, run the git fsck command.[53]
Another property of Git is that it snapshots directory trees of files. The earliest systems for tracking versions of source code, Source Code Control System (SCCS) and Revision Control System (RCS), worked on individual files and emphasized the space savings to be gained from interleaved deltas (SCCS) or delta encoding (RCS) the (mostly similar) versions. Later revision-control systems maintained this notion of a file having an identity across multiple revisions of a project. However, Torvalds rejected this concept.[54] Consequently, Git does not explicitly record file revision relationships at any level below the source-code tree.

These implicit revision relationships have some significant consequences:

It is slightly more costly to examine the change history of one file than the whole project.[55] To obtain a history of changes affecting a given file, Git must walk the global history and then determine whether each change modified that file. This method of examining history does, however, let Git produce with equal efficiency a single history showing the changes to an arbitrary set of files. For example, a subdirectory of the source tree plus an associated global header file is a very common case.
Renames are handled implicitly rather than explicitly. A common complaint with CVS is that it uses the name of a file to identify its revision history, so moving or renaming a file is not possible without either interrupting its history or renaming the history and thereby making the history inaccurate. Most post-CVS revision-control systems solve this by giving a file a unique long-lived name (analogous to an inode number) that survives renaming. Git does not record such an identifier, and this is claimed as an advantage.[56][57] Source code files are sometimes split or merged, or simply renamed,[58] and recording this as a simple rename would freeze an inaccurate description of what happened in the (immutable) history. Git addresses the issue by detecting renames while browsing the history of snapshots rather than recording it when making the snapshot.[59] (Briefly, given a file in revision N, a file of the same name in revision N − 1 is its default ancestor. However, when there is no like-named file in revision N − 1, Git searches for a file that existed only in revision N − 1 and is very similar to the new file.) However, it does require more CPU-intensive work every time the history is reviewed, and several options to adjust the heuristics are available. This mechanism does not always work; sometimes a file that is renamed with changes in the same commit is read as a deletion of the old file and the creation of a new file. Developers can work around this limitation by committing the rename and the changes separately.
Git implements several merging strategies; a non-default strategy can be selected at merge time:[60]

resolve: the traditional three-way merge algorithm.
recursive: This is the default when pulling or merging one branch, and is a variant of the three-way merge algorithm.
When there are more than one common ancestors that can be used for a three-way merge, it creates a merged tree of the common ancestors and uses that as the reference tree for the three-way merge. This has been reported to result in fewer merge conflicts without causing mis-merges by tests done on prior merge commits taken from Linux 2.6 kernel development history. Also, this can detect and handle merges involving renames.

— Linus Torvalds[61]
octopus: This is the default when merging more than two heads.
Data structures[edit]
Git's primitives are not inherently a source-code management system. Torvalds explains:[62]

In many ways you can just see git as a filesystem—it's content-addressable, and it has a notion of versioning, but I really designed it coming at the problem from the viewpoint of a filesystem person (hey, kernels is what I do), and I actually have absolutely zero interest in creating a traditional SCM system.
From this initial design approach, Git has developed the full set of features expected of a traditional SCM,[40] with features mostly being created as needed, then refined and extended over time.


Some data flows and storage levels in the Git revision control system
Git has two data structures: a mutable index (also called stage or cache) that caches information about the working directory and the next revision to be committed; and an immutable, append-only object database.

The index serves as a connection point between the object database and the working tree.

The object database contains five types of objects:[63][53]

A blob (binary large object) is the content of a file. Blobs have no proper file name, time stamps, or other metadata (A blob's name internally is a hash of its content.[64]). In git each blob is a version of a file, it holds the file's data.
A tree object is the equivalent of a directory. It contains a list of file names, each with some type bits and a reference to a blob or tree object that is that file, symbolic link, or directory's contents. These objects are a snapshot of the source tree. (In whole, this comprises a Merkle tree, meaning that only a single hash for the root tree is sufficient and actually used in commits to precisely pinpoint to the exact state of whole tree structures of any number of sub-directories and files.)
A commit object links tree objects together into history. It contains the name of a tree object (of the top-level source directory), a timestamp, a log message, and the names of zero or more parent commit objects.
A tag object is a container that contains a reference to another object and can hold added meta-data related to another object. Most commonly, it is used to store a digital signature of a commit object corresponding to a particular release of the data being tracked by Git.
A packfile object is a zlib version compressed of various other objects for compactness and ease of transport over network protocols.
Each object is identified by a SHA-1 hash of its contents. Git computes the hash and uses this value for the object's name. The object is put into a directory matching the first two characters of its hash. The rest of the hash is used as the file name for that object.

Git stores each revision of a file as a unique blob. The relationships between the blobs can be found through examining the tree and commit objects. Newly added objects are stored in their entirety using zlib compression. This can consume a large amount of disk space quickly, so objects can be combined into packs, which use delta compression to save space, storing blobs as their changes relative to other blobs.

Additionally, git stores labels called refs (short for references) to indicate the locations of various commits. They are stored in the reference database and are respectively:[65]

Heads (branches): Named references that are advanced automatically to the new commit when a commit is made on top of them.
HEAD: A reserved head that will be compared against the working tree to create a commit.
Tags: Like branch references but fixed to a particular commit. Used to label important points in history.
References[edit]
Every object in the Git database that is not referred to may be cleaned up by using a garbage collection command or automatically. An object may be referenced by another object or an explicit reference. Git knows different types of references. The commands to create, move, and delete references vary. "git show-ref" lists all references. Some types are:

heads: refers to an object locally,
remotes: refers to an object which exists in a remote repository,
stash: refers to an object not yet committed,
meta: e.g. a configuration in a bare repository, user rights; the refs/meta/config namespace was introduced retrospectively, gets used by Gerrit,[66]
tags: see above.
Implementations[edit]

gitg is a graphical front-end using GTK+.
Git (the main implementation in C) is primarily developed on Linux, although it also supports most major operating systems, including the BSDs (DragonFly BSD, FreeBSD, NetBSD, and OpenBSD), Solaris, macOS, and Windows.[67][68]

The first Windows port of Git was primarily a Linux-emulation framework that hosts the Linux version. Installing Git under Windows creates a similarly named Program Files directory containing the Mingw-w64 port of the GNU Compiler Collection, Perl 5, MSYS2 (itself a fork of Cygwin, a Unix-like emulation environment for Windows) and various other Windows ports or emulations of Linux utilities and libraries. Currently, native Windows builds of Git are distributed as 32- and 64-bit installers.[69] The git official website currently maintains a build of Git for Windows, still using the MSYS2 environment.[70]

The JGit implementation of Git is a pure Java software library, designed to be embedded in any Java application. JGit is used in the Gerrit code-review tool, and in EGit, a Git client for the Eclipse IDE.[71]

Go-git is an open-source implementation of Git written in pure Go.[72] It is currently used for backing projects as a SQL interface for Git code repositories[73] and providing encryption for Git.[74]

The Dulwich implementation of Git is a pure Python software component for Python 2.7, 3.4 and 3.5.[75]

The libgit2 implementation of Git is an ANSI C software library with no other dependencies, which can be built on multiple platforms, including Windows, Linux, macOS, and BSD.[76] It has bindings for many programming languages, including Ruby, Python, and Haskell.[77][78][79]

JS-Git is a JavaScript implementation of a subset of Git.[80]

Git server[edit]

Screenshot of Gitweb interface showing a commit diff
As Git is a distributed version-control system, it could be used as a server out of the box. It's shipped with a built-in command git daemon which starts a simple TCP server running on the GIT protocol.[81] Dedicated Git HTTP servers help (amongst other features) by adding access control, displaying the contents of a Git repository via the web interfaces, and managing multiple repositories. Already existing Git repositories can be cloned and shared to be used by others as a centralized repo. It can also be accessed via remote shell just by having the Git software installed and allowing a user to log in.[82] Git servers typically listen on TCP port 9418.[83]

Open source[edit]
Hosting the Git server using the Git Binary.[84]
Gerrit, a Git server configurable to support code reviews and providing access via ssh, an integrated Apache MINA or OpenSSH, or an integrated Jetty web server. Gerrit provides integration for LDAP, Active Directory, OpenID, OAuth, Kerberos/GSSAPI, X509 https client certificates. With Gerrit 3.0 all configurations will be stored as Git repositories, no database is required to run. Gerrit has a pull-request feature implemented in its core but lacks a GUI for it.
Phabricator, a spin-off from Facebook. As Facebook primarily uses Mercurial, the Git support is not as prominent.[85]
RhodeCode Community Edition (CE), supporting Git, Mercurial and Subversion with an AGPLv3 license.
Kallithea, supporting both Git and Mercurial, developed in Python with GPL license.
External projects like gitolite,[86] which provide scripts on top of Git software to provide fine-grained access control.
There are several other FLOSS solutions for self-hosting, including Gogs[87] and Gitea, a fork of Gogs, both developed in Go language with MIT license.
Git server as a service[edit]
See also: Comparison of source-code-hosting facilities
There are many offerings of Git repositories as a service. The most popular are GitHub, SourceForge, Bitbucket and GitLab.[88][89][90][91][92]

Adoption[edit]
The Eclipse Foundation reported in its annual community survey that as of May 2014, Git is now the most widely used source-code management tool, with 42.9% of professional software developers reporting that they use Git as their primary source-control system[93] compared with 36.3% in 2013, 32% in 2012; or for Git responses excluding use of GitHub: 33.3% in 2014, 30.3% in 2013, 27.6% in 2012 and 12.8% in 2011.[94] Open-source directory Black Duck Open Hub reports a similar uptake among open-source projects.[95]

Stack Overflow has included version control in their annual developer survey[96] in 2015 (16,694 responses),[97] 2017 (30,730 responses),[98] 2018 (74,298 responses)[99] and 2022 (71,379 reponses).[100] Git was the overwhelming favorite of responding developers in these surveys, reporting as high as 93.9% in 2022.

Version control systems used by responding developers:

Name	2015	2017	2018	2022
Git	69.3%	69.2%	87.2%	93.9%
Subversion	36.9%	9.1%	16.1%	5.2%
TFVC	12.2%	7.3%	10.9%	[ii]
Mercurial	7.9%	1.9%	3.6%	1.1%
CVS	4.2%	[ii]	[ii]	[ii]
Perforce	3.3%	[ii]	[ii]	[ii]
VSS	[ii]	0.6%	[ii]	[ii]
ClearCase	[ii]	0.4%	[ii]	[ii]
Zip file backups	[ii]	2.0%	7.9%	[ii]
Raw network sharing	[ii]	1.7%	7.9%	[ii]
Other	5.8%	3.0%	[ii]	[ii]
None	9.3%	4.8%	4.8%	4.3%
The UK IT jobs website itjobswatch.co.uk reports that as of late September 2016, 29.27% of UK permanent software development job openings have cited Git,[101] ahead of 12.17% for Microsoft Team Foundation Server,[102] 10.60% for Subversion,[103] 1.30% for Mercurial,[104] and 0.48% for Visual SourceSafe.[105]

Extensions[edit]
There are many Git extensions, like Git LFS, which started as an extension to Git in the GitHub community and is now widely used by other repositories. Extensions are usually independently developed and maintained by different people, but at some point in the future, a widely used extension can be merged with Git.

Other open-source Git extensions include:

git-annex, a distributed file synchronization system based on Git
git-flow, a set of Git extensions to provide high-level repository operations for Vincent Driessen's branching model
git-machete, a repository organizer & tool for automating rebase/merge/pull/push operations
Microsoft developed the Virtual File System for Git (VFS for Git; formerly Git Virtual File System or GVFS) extension to handle the size of the Windows source-code tree as part of their 2017 migration from Perforce. VFS for Git allows cloned repositories to use placeholders whose contents are downloaded only once a file is accessed.[106]

Conventions[edit]
Git does not impose many restrictions on how it should be used, but some conventions are adopted in order to organize histories, especially those which require the cooperation of many contributors.

The master branch is created by default with git init [107] and is often used as the branch that other changes are merged into.[108] Correspondingly, the default name of the upstream remote is origin and so the name of the default remote branch is origin/master. The use of master as the default branch name is not universally true. Repositories created in GitHub and GitLab will initialize with a main branch instead of master. [109] [110]
Pushed commits should usually not be overwritten, but should rather be reverted[111] (a commit is made on top which reverses the changes to an earlier commit). This prevents shared new commits based on shared commits from being invalid because the commit on which they are based does not exist in the remote. If the commits contain sensitive information, they should be removed, which involves a more complex procedure to rewrite history.
The git-flow[112] workflow and naming conventions are often adopted to distinguish feature specific unstable histories (feature/*), unstable shared histories (develop), production ready histories (main), and emergency patches to released products (hotfix).
Pull requests are not a feature of git, but are commonly provided by git cloud services. A pull request is a request by one user to merge a branch of their repository fork into another repository sharing the same history (called the upstream remote).[113] The underlying function of a pull request is no different than that of an administrator of a repository pulling changes from another remote (the repository that is the source of the pull request). However, the pull request itself is a ticket managed by the hosting server which initiates scripts to perform these actions; it is not a feature of git SCM.
Security[edit]
Git does not provide access-control mechanisms, but was designed for operation with other tools that specialize in access control.[114]

On 17 December 2014, an exploit was found affecting the Windows and macOS versions of the Git client. An attacker could perform arbitrary code execution on a target computer with Git installed by creating a malicious Git tree (directory) named .git (a directory in Git repositories that stores all the data of the repository) in a different case (such as .GIT or .Git, needed because Git does not allow the all-lowercase version of .git to be created manually) with malicious files in the .git/hooks subdirectory (a folder with executable files that Git runs) on a repository that the attacker made or on a repository that the attacker can modify. If a Windows or Mac user pulls (downloads) a version of the repository with the malicious directory, then switches to that directory, the .git directory will be overwritten (due to the case-insensitive trait of the Windows and Mac filesystems) and the malicious executable files in .git/hooks may be run, which results in the attacker's commands being executed. An attacker could also modify the .git/config configuration file, which allows the attacker to create malicious Git aliases (aliases for Git commands or external commands) or modify extant aliases to execute malicious commands when run. The vulnerability was patched in version 2.2.1 of Git, released on 17 December 2014, and announced the next day.[115][116]

Git version 2.6.1, released on 29 September 2015, contained a patch for a security vulnerability (CVE-2015–7545)[117] that allowed arbitrary code execution.[118] The vulnerability was exploitable if an attacker could convince a victim to clone a specific URL, as the arbitrary commands were embedded in the URL itself.[119] An attacker could use the exploit via a man-in-the-middle attack if the connection was unencrypted,[119] as they could redirect the user to a URL of their choice. Recursive clones were also vulnerable since they allowed the controller of a repository to specify arbitrary URLs via the gitmodules file.[119]

Git uses SHA-1 hashes internally. Linus Torvalds has responded that the hash was mostly to guard against accidental corruption, and the security a cryptographically secure hash gives was just an accidental side effect, with the main security being signing elsewhere.[120][^121] Since a demonstration of the SHAttered attack against git in 2017, git was modified to use a SHA-1 variant resistant to this attack. A plan for hash function transition is being written since February 2020.[^122]

Trademark[edit]
"Git" is a registered word trademark of Software Freedom Conservancy under US500000085961336 since 2015-02-03.

Notes[edit]
^ GPL-2.0-only since 2005-04-11. Some parts under compatible licenses such as LGPLv2.1.[6]
^ Jump up to: a b c d e f g h i j k l m n o p q r s Not listed as an option in this survey

References[edit]
^ "Initial revision of "git", the information manager from hell". GitHub. 8 April 2005. Archived from the original on 16 November 2015. Retrieved 20 December 2015.
^ "Commit Graph". GitHub. 8 June 2016. Archived from the original on 20 January 2016. Retrieved 19 December 2015.
^ "[ANNOUNCE] Git v2.38.0". 3 October 2022. Retrieved 4 October 2022.
^ "Git website". Archived from the original on 9 June 2022. Retrieved 9 June 2022.
^ "Git Source Code Mirror". GitHub. Archived from the original on 3 June 2022. Retrieved 9 June 2022.
^ "Git's LGPL license at github.com". GitHub. 20 May 2011. Archived from the original on 11 April 2016. Retrieved 12 October 2014.
^ "Git's GPL license at github.com". GitHub. 18 January 2010. Archived from the original on 11 April 2016. Retrieved 12 October 2014.
^ "Tech Talk: Linus Torvalds on git (at 00:01:30)". Archived from the original on 20 December 2015. Retrieved 20 July 2014 – via YouTube.
^ Jump up to: a b Torvalds, Linus (7 April 2005). "Re: Kernel SCM saga." linux-kernel (Mailing list). Archived from the original on 1 July 2019. Retrieved 3 February 2017. "So I'm writing some scripts to try to track things a whole lot faster."
^ Jump up to: a b Torvalds, Linus (10 June 2007). "Re: fatal: serious inflate inconsistency". git (Mailing list).
^ Jump up to: a b c d Linus Torvalds (3 May 2007). Google tech talk: Linus Torvalds on git. Event occurs at 02:30. Archived from the original on 28 May 2007. Retrieved 16 May 2007.
^ "A Short History of Git". Pro Git (2nd ed.). Apress. 2014. Archived from the original on 25 December 2015. Retrieved 26 December 2015.
^ Chacon, Scott (24 December 2014). Pro Git (2nd ed.). New York, NY: Apress. pp. 29–30. ISBN 978-1-4842-0077-3. Archived from the original on 25 December 2015.
^ Brown, Zack (27 July 2018). "A Git Origin Story". Linux Journal. Linux Journal. Archived from the original on 13 April 2020. Retrieved 28 May 2020.
^ BitKeeper and Linux: The end of the road? Archived 8 June 2017 at the Wayback Machine
^ McAllister, Neil (2 May 2005). "Linus Torvalds' BitKeeper blunder". InfoWorld. Archived from the original on 26 August 2015. Retrieved 8 September 2015.
^ Jump up to: a b Torvalds, Linus (27 February 2007). "Re: Trivia: When did git self-host?". git (Mailing list).
^ Torvalds, Linus (6 April 2005). "Kernel SCM saga." linux-kernel (Mailing list).
^ Torvalds, Linus (17 April 2005). "First ever real kernel git merge!". git (Mailing list).
^ Mackall, Matt (29 April 2005). "Mercurial 0.4b vs git patchbomb benchmark". git (Mailing list).
^ Torvalds, Linus (17 June 2005). "Linux 2.6.12". git-commits-head (Mailing list).
^ Torvalds, Linus (27 July 2005). "Meet the new maintainer..." git (Mailing list).
^ Hamano, Junio C. (21 December 2005). "Announce: Git 1.0.0". git (Mailing list).
^ "GitFaq: Why the 'Git' name?". Git.or.cz. Archived from the original on 23 July 2012. Retrieved 14 July 2012.
^ "After controversy, Torvalds begins work on 'git'". PC World. 14 July 2012. Archived from the original on 1 February 2011. Torvalds seemed aware that his decision to drop BitKeeper would also be controversial. When asked why he called the new software, 'git', British slang meaning 'a rotten person', he said. 'I'm an egotistical bastard, so I name all my projects after myself. First Linux, now git.'
^ "git(1) Manual Page". Archived from the original on 21 June 2012. Retrieved 21 July 2012.
^ "Initial revision of 'git', the information manager from hell · git/git@e83c516". GitHub. Archived from the original on 8 October 2017. Retrieved 21 January 2016.
^ "Tags". GitHub. Archived from the original on 29 September 2021. Retrieved 28 January 2022.
^ "Highlights from Git 2.25". The GitHub Blog. 13 January 2020. Archived from the original on 22 March 2021. Retrieved 27 November 2020. A sparse checkout is nothing more than a list of file path patterns that Git should attempt to populate in your working copy when checking out the contents of your repository. Effectively, it works like a .gitignore, except it acts on the contents of your working copy, rather than on your index.
^ "Highlights from Git 2.26". The GitHub Blog. 22 March 2020. Archived from the original on 22 March 2021. Retrieved 25 November 2020. You may remember when Git introduced a new version of its network fetch protocol way back in 2018. That protocol is now used by default in 2.26, so let’s refresh ourselves on what that means. The biggest problem with the old protocol is that the server would immediately list all of the branches, tags, and other references in the repository before the client had a chance to send anything. For some repositories, this could mean sending megabytes of extra data, when the client really only wanted to know about the master branch. The new protocol starts with the client request and provides a way for the client to tell the server which references it’s interested in. Fetching a single branch will only ask about that branch, while most clones will only ask about branches and tags. This might seem like everything, but server repositories may store other references (such as the head of every pull request opened in the repository since its creation). Now, fetches from large repositories improve in speed, especially when the fetch itself is small, which makes the cost of the initial reference advertisement more expensive relatively speaking. And the best part is that you won’t need to do anything! Due to some clever design, any client that speaks the new protocol can work seamlessly with both old and new servers, falling back to the original protocol if the server doesn’t support it. The only reason for the delay between introducing the protocol and making it the default was to let early adopters discover any bugs.
^ "Highlights from Git 2.28". The GitHub Blog. 27 July 2020. Archived from the original on 22 March 2021. Retrieved 25 November 2020.
^ "Highlights from Git 2.29". The GitHub Blog. 19 October 2020. Archived from the original on 22 March 2021. Retrieved 25 November 2020.
^ "Git 2.30 Release Notes". Git Downloads. 27 December 2020. Archived from the original on 22 March 2021. Retrieved 27 December 2020.
^ "Git 2.31 Release Notes". Git Downloads. 3 April 2021. Retrieved 3 April 2021.
^ "Highlights from Git 2.31". The GitHub Blog. 15 March 2021. Retrieved 2 July 2021.
^ "git/git". GitHub. Archived from the original on 8 February 2017. Retrieved 1 December 2011.
^ Hamano, Junio (21 November 2007). "How to maintain Git". GitHub. Archived from the original on 22 March 2021. Retrieved 1 August 2020.
^ Torvalds, Linus (5 May 2006). "Re: [ANNOUNCE] Git wiki". linux-kernel (Mailing list). "Some historical background" on Git's predecessors
^ Jump up to: a b Torvalds, Linus (8 April 2005). "Re: Kernel SCM saga". linux-kernel (Mailing list). Archived from the original on 22 March 2021. Retrieved 20 February 2008.
^ Jump up to: a b Torvalds, Linus (23 March 2006). "Re: Errors GITtifying GCC and Binutils". git (Mailing list). Archived from the original on 22 March 2021. Retrieved 3 February 2017.
^ Torvalds, Linus (20 October 2006). "Re: VCS comparison table". git (Mailing list). Archived from the original on 22 March 2021. Retrieved 3 February 2017. A discussion of Git vs. BitKeeper.
^ "Git – A Short History of Git". git-scm.com. Archived from the original on 22 March 2021. Retrieved 15 June 2020.
^ "Git – Distributed Workflows". git-scm.com. Archived from the original on 22 October 2014. Retrieved 15 June 2020.
^ Gunjal, Siddhesh (19 July 2019). "What is Version Control Tool? Explore Git and GitHub". Medium. Retrieved 25 October 2020.
^ Torvalds, Linus (19 October 2006). "Re: VCS comparison table". git (Mailing list).
^ Jst's Blog on Mozillazine "bzr/hg/git performance". Archived from the original on 29 May 2010. Retrieved 12 February 2015.
^ Dreier, Roland (13 November 2006). "Oh what a relief it is". Archived from the original on 16 January 2009., observing that "git log" is 100x faster than "svn log" because the latter must contact a remote server.
^ "Trust". Git Concepts. Git User's Manual. 18 October 2006. Archived from the original on 22 February 2017.
^ Torvalds, Linus. "Re: VCS comparison table". git (Mailing list). Retrieved 10 April 2009., describing Git's script-oriented design
^ iabervon (22 December 2005). "Git rocks!". Archived from the original on 14 September 2016., praising Git's scriptability.
^ "Git – Git SCM Wiki". git.wiki.kernel.org. Retrieved 25 October 2020.
^ "Git User's Manual". 10 March 2020. Archived from the original on 10 May 2020.
^ Jump up to: a b "Git – Packfiles". git-scm.com.
^ Torvalds, Linus (10 April 2005). "Re: more git updates." linux-kernel (Mailing list).
^ Haible, Bruno (11 February 2007). "how to speed up 'git log'?". git (Mailing list).
^ Torvalds, Linus (1 March 2006). "Re: impure renames / history tracking". git (Mailing list).
^ Hamano, Junio C. (24 March 2006). "Re: Errors GITtifying GCC and Binutils". git (Mailing list).
^ Hamano, Junio C. (23 March 2006). "Re: Errors GITtifying GCC and Binutils". git (Mailing list).
^ Torvalds, Linus (28 November 2006). "Re: git and bzr". git (Mailing list)., on using git-blame to show code moved between source files.
^ Torvalds, Linus (18 July 2007). "git-merge(1)". Archived from the original on 16 July 2016.
^ Torvalds, Linus (18 July 2007). "CrissCrossMerge". Archived from the original on 13 January 2006.
^ Torvalds, Linus (10 April 2005). "Re: more git updates..." linux-kernel (Mailing list).
^ "Git – Git Objects". git-scm.com.
^ Some of git internals
^ "Git – Git References". git-scm.com.
^ "Project Configuration File Format". Gerrit Code Review. Archived from the original on 3 December 2020. Retrieved 2 February 2020.
^ "downloads". Archived from the original on 8 May 2012. Retrieved 14 May 2012.
^ "git package versions – Repology". Archived from the original on 19 January 2022. Retrieved 30 November 2021.
^ "msysGit". GitHub. Archived from the original on 10 October 2016. Retrieved 20 September 2016.
^ "Git – Downloading Package". git-scm.com. (source code)
^ "JGit". Archived from the original on 31 August 2012. Retrieved 24 August 2012.
^ "Git – go-git". git-scm.com. Retrieved 19 April 2019.
^ "SQL interface to Git repositories, written in Go.", github.com, retrieved 19 April 2019
^ "Keybase launches encrypted git". keybase.io. Retrieved 19 April 2019.
^ "Dulwich". Archived from the original on 29 May 2012. Retrieved 27 August 2012.
^ "libgit2". GitHub. Archived from the original on 11 April 2016. Retrieved 24 August 2012.
^ "rugged". GitHub. Archived from the original on 24 July 2013. Retrieved 24 August 2012.
^ "pygit2". GitHub. Archived from the original on 5 August 2015. Retrieved 24 August 2012.
^ "hlibgit2". Archived from the original on 25 May 2013. Retrieved 30 April 2013.
^ "js-git: a JavaScript implementation of Git". GitHub. Archived from the original on 7 August 2013. Retrieved 13 August 2013.
^ "Git – Git Daemon". git-scm.com. Retrieved 10 July 2019.
^ 4.4 Git on the Server – Setting Up the Server Archived 22 October 2014 at the Wayback Machine, Pro Git.
^ "1.4 Getting Started – Installing Git". git-scm.com. Archived from the original on 2 November 2013. Retrieved 1 November 2013.
^ Chacon, Scott; Straub, Ben (2014). "Git on the Server – Setting Up the Server". Pro Git (2nd ed.). Apress. ISBN 978-1484200773.
^ Diffusion User Guide: Repository Hosting Archived 20 September 2020 at the Wayback Machine.
^ "Gitolite: Hosting Git Repositories".
^ "Gogs: A painless self-hosted Git service".
^ Krill, Paul (28 September 2016). "Enterprise repo wars: GitHub vs. GitLab vs. Bitbucket". InfoWorld. Retrieved 2 February 2020.
^ "github.com Competitive Analysis, Marketing Mix and Traffic". Alexa. Archived from the original on 31 March 2013. Retrieved 2 February 2020.
^ "sourceforge.net Competitive Analysis, Marketing Mix and Traffic". Alexa. Archived from the original on 20 October 2020. Retrieved 2 February 2020.
^ "bitbucket.org Competitive Analysis, Marketing Mix and Traffic". Alexa. Archived from the original on 23 June 2017. Retrieved 2 February 2020.
^ "gitlab.com Competitive Analysis, Marketing Mix and Traffic". Alexa. Archived from the original on 30 November 2017. Retrieved 2 February 2020.
^ "Eclipse Community Survey 2014 results | Ian Skerrett". Ianskerrett.wordpress.com. 23 June 2014. Archived from the original on 25 June 2014. Retrieved 23 June 2014.
^ "Results of Eclipse Community Survey 2012". eclipse.org. Archived from the original on 11 April 2016.
^ "Compare Repositories – Open Hub". Archived from the original on 7 September 2014.
^ "Stack Overflow Annual Developer Survey". Stack Exchange, Inc. Retrieved 9 January 2020. Stack Overflow’s annual Developer Survey is the largest and most comprehensive survey of people who code around the world. Each year, we field a survey covering everything from developers' favorite technologies to their job preferences. This year marks the ninth year we’ve published our annual Developer Survey results, and nearly 90,000 developers took the 20-minute survey earlier this year.
^ "Stack Overflow Developer Survey 2015". Stack Overflow. Archived from the original on 4 May 2019. Retrieved 29 May 2019.
^ "Stack Overflow Developer Survey 2017". Stack Overflow. Archived from the original on 29 May 2019. Retrieved 29 May 2019.
^ "Stack Overflow Developer Survey 2018". Stack Overflow. Archived from the original on 30 May 2019. Retrieved 29 May 2019.
^ "Stack Overflow Developer Survey 2022". Stack Overflow. Retrieved 4 August 2022.
^ "Git (software) Jobs, Average Salary for Git Distributed Version Control System Skills". Itjobswatch.co.uk. Archived from the original on 8 October 2016. Retrieved 30 September 2016.
^ "Team Foundation Server Jobs, Average Salary for Microsoft Team Foundation Server (TFS) Skills". Itjobswatch.co.uk. Archived from the original on 29 October 2016. Retrieved 30 September 2016.
^ "Subversion Jobs, Average Salary for Apache Subversion (SVN) Skills". Itjobswatch.co.uk. Archived from the original on 25 October 2016. Retrieved 30 September 2016.
^ "Mercurial Jobs, Average Salary for Mercurial Skills". Itjobswatch.co.uk. Archived from the original on 23 September 2016. Retrieved 30 September 2016.
^ "VSS/SourceSafe Jobs, Average Salary for Microsoft Visual SourceSafe (VSS) Skills". Itjobswatch.co.uk. Archived from the original on 29 October 2016. Retrieved 30 September 2016.
^ "Windows switch to Git almost complete: 8,500 commits and 1,760 builds each day". Ars Technica. 24 May 2017. Archived from the original on 24 May 2017. Retrieved 24 May 2017.
^ "git-init". Git. Archived from the original on 15 March 2022.
^ "Git – Branches in a Nutshell". git-scm.com. Archived from the original on 20 December 2020. Retrieved 15 June 2020. The "master" branch in Git is not a special branch. It is exactly like any other branch. The only reason nearly every repository has one is that the git init command creates it by default and most people don’t bother to change it.
^ github/renaming, GitHub, 4 December 2020, retrieved 4 December 2020
^ Default branch name for new repositories now main, GitLab, 22 June 2021, retrieved 22 June 2021
^ "Git Revert | Atlassian Git Tutorial". Atlassian. Reverting has two important advantages over resetting. First, it doesn’t change the project history, which makes it a "safe" operation for commits that have already been published to a shared repository.
^ "Gitflow Workflow | Atlassian Git Tutorial". Atlassian. Retrieved 15 June 2020.
^ "Forking Workflow | Atlassian Git Tutorial". Atlassian. Retrieved 15 June 2020.
^ "Git repository access control". Archived from the original on 14 September 2016. Retrieved 6 September 2016.
^ Pettersen, Tim (20 December 2014). "Securing your Git server against CVE-2014-9390". Archived from the original on 24 December 2014. Retrieved 22 December 2014.
^ Hamano, J. C. (18 December 2014). "[Announce] Git v2.2.1 (and updates to older maintenance tracks)". Newsgroup: gmane.linux.kernel. Archived from the original on 19 December 2014. Retrieved 22 December 2014.
^ "CVE-2015-7545". 15 December 2015. Archived from the original on 26 December 2015. Retrieved 26 December 2015.
^ "Git 2.6.1". GitHub. 29 September 2015. Archived from the original on 11 April 2016. Retrieved 26 December 2015.
^ Jump up to: a b c Blake Burkhart; et al. (5 October 2015). "Re: CVE Request: git". Archived from the original on 27 December 2015. Retrieved 26 December 2015.
^ "hash – How safe are signed git tags? Only as safe as SHA-1 or somehow safer?". Information Security Stack Exchange. 22 September 2014. Archived from the original on 24 June 2016.
- [^121] "Why does Git use a cryptographic hash function?". Stack Overflow. 1 March 2015. Archived from the original on 1 July 2016.
- [^122]: "Git – hash-function-transition Documentation". git-scm.com.

External links[edit]

Wikimedia Commons has media related to Git.

Wikibooks has a book on the topic of: Git
Official website Edit this at Wikidata
Git at Open Hub
