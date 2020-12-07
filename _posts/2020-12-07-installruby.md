---
layout: post
title: EC2 인스턴스에 ruby on rails 설치하기
tags:
  - ruby
  - ec2
---

```bash
yum -y update
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
cd ~/.rbenv
src/configure
make -C src # openssl 등의 패키지가 없어서 설치가 안 될 경우는 해당 패키지를 설치함
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
.rbenv/bin/rbenv init
rbenv install 2.7.2
rbenv global 2.7.2
gem install bundler
gem install rails
```
