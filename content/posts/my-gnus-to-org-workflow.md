+++
title = "My (Bare-bones) Gnus to Org workflow"
author = ["myself"]
date = 2018-05-20T22:36:00-03:00
categories = ["emacs"]
draft = false
+++

## Introduction {#introduction}

As a project starts to grow more and more complex communication-wise, we can notice the amount of tools involved under the task management umbrella increasing; since each client and even our own team has a predilection for a certain way of working. Thankfully email is a funnel we can rely on and use as a way to standarize our own queue. Emacs is super useful as a task planner through org-mode, and the question is "how can we avoid context switching from email to org?".


## Procedure {#procedure}


### Set Up {#set-up}

First, we need to add the following lines to our **`~/.emacs`** file. In this example we use the FastMail IMAP server.

```emacs-lisp
(setq epa-pinentry-mode 'loopback)  ; this one is critical, actually

(setq gnus-select-method
      '(nnimap "fastmail"
               (nnimap-address "imap.fastmail.com")
               (nnimap-server-port "imaps")
               (nnimap-stream ssl)))
```

As you can notice we do not specify our credentials here, which is good since we will be using an encrypted file to store them. Before creating said file (which will actually hold our username and password) we must generate the GPG secret that will allow us to perform the encryption.

```shell
$ gpg --gen-key
```

That's all the time we'll be spending in the terminal. Head back to emacs and visit the file named **`~/.authinfo.gpg`** and write the following line.

```shell
machine imap.fastmail.com login <USER> password <APP-PASSWORD> port imaps
```

Save the file and in the popup window select the secret we just created to perform the encryption.


### Using Gnus {#using-gnus}

Press **`M-x gnus`** and you will be prompted for the GPG secret you just created. Hit **`RET`**. After doing so you may or may not see a thing depending on the current status of your inbox. This is the [group view](https://www.gnu.org/software/emacs/manual/html_node/gnus/Listing-Groups.html#Listing-Groups) and here you will be shown all the groups with unread messages.
Press L to see all your groups (a.k.a Folders if we are talking about email) and press l (lowercase L) to see all the groups/folders with unread messages.
Now you can visit any of them and start performing the usual mail tasks by pressing RET. Go ahead and visit an email.
You are in the [summary buffer](https://www.gnu.org/software/emacs/manual/html_node/gnus/Summary-Buffer.html#Summary-Buffer) and this represents the email tray as you know it. There are two things that can happen.

1.  You will only see the unread messages.
2.  You will see all the read ones.

Anyhow, you can start toying with the messages.


#### Marking articles {#marking-articles}

All the marking commands understand the numeric prefix.

| Command        | Action                                                                                                                                                          |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ‘M c’          | Clear all readedness-marks from the current article                                                                                                             |
| ‘M-u’          | (‘gnus-summary-clear-mark-forward’).  In other words, mark the article as unread.                                                                               |
| ‘M t’ or ‘!’   | Tick the current article (‘gnus-summary-tick-article-forward’).                                                                                                 |
| ‘M ?’ or ‘?’   | Mark the current article as dormant (‘gnus-summary-mark-as-dormant’).                                                                                           |
| ‘M d’ or ‘d’   | Mark the current article as read (‘gnus-summary-mark-as-read-forward’).                                                                                         |
| ‘D’            | Mark the current article as read and move point to the previous line (‘gnus-summary-mark-as-read-backward’).                                                    |
| ‘M k’ or ‘k’   | Mark all articles that have the same subject as the current one as read, and then select the next unread article (‘gnus-summary-kill-same-subject-and-select’). |
| ‘M K’ or ‘C-k’ | Mark all articles that have the same subject as the current one as read (‘gnus-summary-kill-same-subject’).                                                     |
| ‘M C’          | Mark all unread articles as read (‘gnus-summary-catchup’).                                                                                                      |
| ‘M C-c’        | Mark all articles in the group as read—even the ticked and dormant articles (‘gnus-summary-catchup-all’).                                                       |
| ‘M H’          | Catchup the current group to point (before the point) (‘gnus-summary-catchup-to-here’).                                                                         |
| ‘M h’          | Catchup the current group from point (after the point) (‘gnus-summary-catchup-from-here’).                                                                      |
| ‘C-w’          | Mark all articles between point and mark as read (‘gnus-summary-mark-region-as-read’).                                                                          |
| ‘M V k’        | Kill all articles with scores below the default score (or below the numeric prefix) (‘gnus-summary-kill-below’).                                                |
| ‘M e’ or ‘E’   | Mark the current article as expirable (‘gnus-summary-mark-as-expirable’).                                                                                       |
| ‘M b’          | Set a bookmark in the current article (‘gnus-summary-set-bookmark’).                                                                                            |
| ‘M B’          | Remove the bookmark from the current article (‘gnus-summary-remove-bookmark’).                                                                                  |
| ‘M V c’        | Clear all marks from articles with scores over the default score (or over the numeric prefix) (‘gnus-summary-clear-above’).                                     |
| ‘M V u’        | Tick all articles with scores over the default score (or over the numeric prefix) (‘gnus-summary-tick-above’).                                                  |
| ‘M V m’        | Prompt for a mark, and mark all articles with scores over the default score (or over the numeric prefix) with this mark (‘gnus-summary-clear-above’).           |


#### Moving email around {#moving-email-around}

We can even perform the [standard email administration tasks](https://www.gnu.org/software/emacs/manual/html_node/gnus/Mail-Group-Commands.html) such as moving and copying messages.

| Command | Action                                                                                                                                                                             |
|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| B m     | Move the article from one mail group to another (gnus-summary-move-article). Marks will be preserved if gnus-preserve-marks is non-nil (which is the default).                     |
| B c     | Copy the article from one group (mail group or not) to a mail group (gnus-summary-copy-article). Marks will be preserved if gnus-preserve-marks is non-nil (which is the default). |


#### Capturing in Org-mode {#capturing-in-org-mode}

I was about to give up on this, until I decided to scroll down on the [template expansion](https://orgmode.org/manual/Template-expansion.html#FOOT91) page from the org-mode guide. It's amazing how instructive paying attention can be. SMH.

| Link type     | Available keywords                                     |
|---------------|--------------------------------------------------------|
| gnus, notmuch | %:from %:fromname %:fromaddress                        |
|               | %:to   %:toname   %:toaddress                          |
|               | %:date (message date header field)                     |
|               | %:date-timestamp (date as active timestamp)            |
|               | %:date-timestamp-inactive (date as inactive timestamp) |
|               | %:fromto (either "to NAME" or "from NAME")(92)         |
| gnus          | %:group, for messages also all email fields            |

Here's an example on how you could create a simple template using the full link to the email (%l) and the subject (%:subject) to generate a nice TODO entry.

```emacs-lisp
(org-capture-templates
   (quote
    (("t" "Todo")
     ("tm" "Mail" entry
      (file+headline "~/Dropbox/Private/ORG/inbox.org" "INBOX")
      "* TODO [[%l][%:subject]]"))))
```


## Conclusion {#conclusion}

At the time I am just starting to figure out this wokflow, but it feels promising enough to document the required steps therefore subsequently making it easier for the newcomer to try it out.


## Credits {#credits}


### References {#references}

-   [Emacs 25 EasyPG Issue](https://colinxy.github.io/software-installation/2016/09/24/emacs25-easypg-issue.html)
-   [Gnus on EmacsWiki](https://www.emacswiki.org/emacs/GnusGmail)
