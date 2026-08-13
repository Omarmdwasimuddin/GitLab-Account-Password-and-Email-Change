# GitLab Account: Password and Email Change

####
```bash
docker exec -it my-gitlab-server gitlab-rails console
```
---

#### kichukhon wait korle ruby console ashbe
```bash
--------------------------------------------------------------------------------
 Ruby:         ruby 3.x.x
 GitLab:       ...
 Rails:        ...
--------------------------------------------------------------------------------
Loading production environment
irb(main):001>
```
---

#### 
```bash
user = User.find_by_username('root')
```
```bash
user.password = 'w@sIm1997$'
```
```bash
user.password_confirmation = 'w@sIm1997$'
```
```bash
user.save!
```
```bash
exit
```
---
