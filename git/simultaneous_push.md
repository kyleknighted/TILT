In the organization I work in, due to multiple mergers and acquisitions, there are multiple instances of GitHub.
Sometimes I need to contribute something to two, or more, repositories on different instances.

I wanted to simplify the amount of commands I needed to run and came across this!

1. What you'll need to do for a new project is make sure the repository is setup on both GitHubs
1. Whenever you setup a new repo, GitHub provides you with the first few commands to get rolling
1. After the command `git remote add origin https://github.com/Your-Name/Repo-Name.git`
1. Instead of pushing, you will instead run `git remote set-url origin --push --add https://github.com/Your-Second-Name/Repo-Name.git`
1. And now you can continue on to `git push -u origin master` - this will now push your repo to both GitHubs
