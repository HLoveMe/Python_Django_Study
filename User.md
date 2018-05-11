# USer

```
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
	 id
	 password
	 last_login
	 is_superuser
	 username
	 first_name
	 last_name
	 email
	 is_staff
	 is_active
	 date_joined
    nick_name = models.CharField(
        max_length=50,
        verbose_name=u"昵称",
        default="",
        null=False,
        blank=False
    )
    
Setting 配置

makemigrations Users
migrate
//yes 删除
    
```