# To configure telegram auth reproduce this steps:

## Telegram steps
1. Create a new bot in Telegram by talking to the BotFather (https://core.telegram.org/bots#botfather)
2. Copy the token that the BotFather gives you
3. Paste the token in the `SOCIAL_AUTH_TELEGRAM_BOT_TOKEN` setting in the `settings.py`
file (prefer to get it from env or other secret management mechanic, this project is just for demo purposes)

## Django steps
1. Install drf-social-oauth2 
```bash
poetry add drf-social-oauth2
```
2. Add to INSTALLED_APPS in settings.py
```python
INSTALLED_APPS = [
    ...
    'oauth2_provider',
    'social_django',
    'drf_social_oauth2',
    ...
]
```
3. Add to templates in settings.py
```python
TEMPLATES = [
    {
        ...
        'OPTIONS': {
            'context_processors': [
                ...
                'social_django.context_processors.backends',
                'social_django.context_processors.login_redirect',
                ...
            ],
        },
    },
]
```
4. Add to AUTHENTICATION_BACKENDS in settings.py
```python
AUTHENTICATION_BACKENDS = (
    'social_core.backends.telegram.TelegramAuth',
    'drf_social_oauth2.backends.DjangoOAuth2',
    'django.contrib.auth.backends.ModelBackend',
)
```
5. Configure REST_FRAMEWORK in settings.py
```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'drf_social_oauth2.authentication.SocialAuthentication',
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.BasicAuthentication',
    ),
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
}
```
Optional: if you are using corsheaders add to MIDDLEWARE in settings.py
```python
SECURE_CROSS_ORIGIN_OPENER_POLICY = "same-origin-allow-popups"
```

# Frontend steps
1. Get the frontend code for auth button from https://core.telegram.org/widgets/login
2. Change data-telegram-login to your bot name and data-auth-url to your backend auth url (use ngrok or other tunneling service to test it locally)