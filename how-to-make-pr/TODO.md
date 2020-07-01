
- github is going to stop supporting user/password auth over https in the fall of 2020, so need to add support for auth tokens for those who don't have ssh set up.

cmd="curl -X POST -H \"Authorization: token $token\" https://api.github.com/repos/$repo_user/$repo_name/forks"

- the script currently can't handle the situation where it tries to fork a repository with an already existing repository of the same name - it checks that this repo already exists, and doesn't fork it. e.g. let's make a pr branch of:

https://github.com/kasparlund/nlp

  and then of:

https://github.com/huggingface/nlp

  moreover, when performing fork github will rename the second fork to `nlp-1` (if that name is available), so it's a big mess, since now the script will be very confused as it won't be able to neither sync nor setup the proper git streams. So for now just make sure you don't already have a pre-existing fork with the same name - rename it on github if you have to.
