# Core problem
The `segurança` branch `requirements.txt` was originally using `bcrypt>=4.0.1` which created a compatibility issue between `passlib` and `bcrypt` 4.0+. Brcrypt 4.0.0 removed the `__about__` attribute that passlib relied on.

```bash
sushi-backend-1  | (trapped) error reading bcrypt version
sushi-backend-1  | Traceback (most recent call last):
sushi-backend-1  |   File "/usr/local/lib/python3.11/site-packages/passlib/handlers/bcrypt.py", line 620, in _load_backend_mixin
sushi-backend-1  |     version = _bcrypt.__about__.__version__
sushi-backend-1  |               ^^^^^^^^^^^^^^^^^
sushi-backend-1  | AttributeError: module 'bcrypt' has no attribute '__about__'
sushi-backend-1  | Traceback (most recent call last):
sushi-backend-1  |   File "<frozen runpy>", line 198, in _run_module_as_main
```
## Solution
Switch theses lines in requirements:

```txt
[...]

sqlalchemy>=2.0.23
redis>=6.2.0
python-jose[cryptography]>=3.3.0
passlib>=1.7.4
# bcrypt>=4.0.1 <- Change this version
bcrypt==3.2.2 <- To this version

[...]
```

Because we are using Pydantic `>=2.5.0` that means that the `email-validator`is no longer bundled as a default dependency, so we have to declare it separately in the `requirements.txt`. Therefore, add this to the `requirements.txt`:

```
email-validator>=2.0.0
```

___
