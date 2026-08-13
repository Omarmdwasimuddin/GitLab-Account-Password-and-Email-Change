# GitLab অ্যাডমিন Password ও Email পরিবর্তন

এই ডকুমেন্টে দেখানো হয়েছে কীভাবে `my-gitlab-server` নামের Docker container-এ থাকা GitLab-এর root user-এর password এবং email, Rails console ব্যবহার করে পরিবর্তন করা যায়।

> **প্রস্তুতি:** `Demo` folder-এ (যেখানে GitLab server এবং GitLab Runner-এর ফাইল আছে) terminal open করে নিচের ধাপগুলো অনুসরণ করুন।

---

## ১. Root Password পরিবর্তন

### ধাপ ১: Rails Console চালু করা

```bash
docker exec -it my-gitlab-server gitlab-rails console
```

কিছুক্ষণ wait করলে নিচের মতো console load হবে:

```
--------------------------------------------------------------------------------
 Ruby:         ruby 3.x.x
 GitLab:       ...
 Rails:        ...
--------------------------------------------------------------------------------
Loading production environment
irb(main):001>
```

### ধাপ ২: User খুঁজে বের করা ও Password সেট করা

Console-এ একে একে নিচের command গুলো লিখুন:

```ruby
user = User.find_by_username('root')
user.password = 'w@sIm1997$'
user.password_confirmation = 'w@sIm1997$'
user.password_automatically_set = false
user.save!
```

> **Note:** `password_automatically_set = false` লাইনটি official GitLab documentation-এ recommended — এটা না দিলে অনেক সময় GitLab password validation নিয়ে সমস্যা করে এবং login fail করতে পারে।

সফলভাবে save হলে console output-এ `true` দেখাবে। এরপর console থেকে বের হতে:

```ruby
exit
```

### ধাপ ৩: Login Verify করা

Browser-এ যান:

```
http://localhost:8000
```

Login credentials:

| Field    | Value          |
|----------|----------------|
| Username | `root`         |
| Password | `w@sIm1997$`   |

সফলভাবে login হলে বুঝবেন password change সঠিকভাবে হয়েছে।

---

## ২. Admin Email পরিবর্তন

### ধাপ ১: Rails Console চালু করা

```bash
docker exec -it my-gitlab-server gitlab-rails console
```

আবার console load হওয়ার জন্য কিছুক্ষণ wait করুন।

### ধাপ ২: Email পরিবর্তন করা

```ruby
root = User.find_by_username('root')
root.email = 'mdwasimu015@gmail.com'
root.skip_reconfirmation!
root.save!
```

- `skip_reconfirmation!` ব্যবহার করা হয়েছে যাতে নতুন email confirm করার জন্য আলাদাভাবে confirmation email আসা এবং click করার প্রয়োজন না হয়।
- `save!` ব্যবহার করার কারণে, কোনো validation error হলে সাথে সাথে exception raise হবে (silent fail হবে না) — এটা debugging-এর জন্য ভালো।

### ধাপ ৩: Verify করা

একই console-এ (অথবা নতুন session-এ) নিচের command দিয়ে confirm করুন:

```ruby
User.find_by_username('root').email
```

Output-এ `mdwasimu015@gmail.com` দেখা গেলে বুঝবেন email পরিবর্তন সফল হয়েছে।

---

## গুরুত্বপূর্ণ Security নোট

- এই পুরো process টা শুধুমাত্র **trusted/local environment**-এ করা উচিত, কারণ Rails console-এর মাধ্যমে সরাসরি database-এ access পাওয়া যায় — কোনো authentication বা audit log ছাড়াই।
- Production server-এ এই কাজ করলে অবশ্যই সতর্কতার সাথে করবেন এবং কাজ শেষে অবশ্যই একটা strong, unique password ব্যবহার করবেন।
- Password এবং email এই ডকুমেন্টে plain text আকারে আছে — production use-এর জন্য ফাইলটি secure জায়গায় রাখুন বা কাজ শেষে sensitive value গুলো মুছে ফেলুন।
