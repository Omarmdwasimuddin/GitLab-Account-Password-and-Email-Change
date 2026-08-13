# GitLab Account: Admin Password and Email Change

#### gitlab server o gitlab runner er folder ache Demo folder e. demo folder vs e open kore terminal e daw.

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

#### eke eke daw
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

#### browser e jaw
```bash
http://localhost:8000
```
---

#### login koro
```bash
Username: root
Password: w@sIm1997$
```
---

#### Successfully password change hoye jabe!
---

#### admin email change

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

#### eke eke daw
```bash
root = User.find_by_username('root')
```
```bash
root.email = 'mdwasimu015@gmail.com'
```
```bash
root.skip_reconfirmation!
```
```bash
root.save!
```
---

#### verify koro root email change hoiche kina
```bash
User.find_by_username('root').email
```
---
