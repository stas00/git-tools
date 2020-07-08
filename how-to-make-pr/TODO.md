
- github is going to stop supporting user/password auth over https in the fall of 2020, so need to add support for auth tokens for those who don't have ssh set up.

cmd="curl -X POST -H \"Authorization: token $token\" https://api.github.com/repos/$repo_user/$repo_name/forks"
