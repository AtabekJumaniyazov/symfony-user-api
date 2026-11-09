# Symfony User API Loyihasi (API Platform + JWT)

Ushbu loyiha Symfony va API Platform yordamida foydalanuvchilarni ro'yxatdan o'tkazish (`fullname`, `surname`, `age`, `email`, `password`), JWT token olish va foydalanuvchi ma'lumotlarini boshqarish uchun tayyorlangan.

## Talablar bajarilishi
1. **User Entity (`src/Entity/User.php`)**:
   - `fullname`, `surname`, `age`, `email`, `password` xususiyatlari yaratilgan va ma'lumotlar omborida `nullable: false` qilib belgilangan.
   - `#[ApiResource]` orqali `PATCH` dan tashqari barcha endpointlar (`GetCollection`, `Post`, `Get`, `Put`, `Delete`) va `/api/users/auth` endpointi ulangan.
   - `normalizationContext: ['groups' => ['user:read']]` va `denormalizationContext: ['groups' => ['user:write']]` sozlangan.
   - Foydalanuvchi ro'yxatdan o'tganda kiritgan ma'lumotlari (`id`, `email`, `fullname`, `surname`, `age`) `user:read` guruhi orqali o'ziga ko'rinadi (`password` faqat `user:write` da).
2. **Dizayn qoliplari (Design Patterns)**:
   - **Factory (`src/Component/User/UserFactory.php`)**: Yangi `User` obyektini yaratadi va parolni xeshlaydi (`UserPasswordHasherInterface`).
   - **Manager (`src/Component/User/UserManager.php`)**: `EntityManagerInterface` orqali `persist` va `flush` amallarini bajaradi.
   - **Controller (`src/Controller/UserCreateAction.php`)**: `POST /api/users` so'rovini qabul qilib, `UserFactory` va `UserManager` orqali foydalanuvchini saqlaydi.
3. **JWT va Xavfsizlik (`config/packages/security.yaml`)**:
   - `POST /api/users` (foydalanuvchi yaratish) va `POST /api/users/auth` (token olish) uchun `PUBLIC_ACCESS` berilgan (avtorizatsiya so'ralmaydi).
   - Qolgan barcha `/api` endpointlari uchun `IS_AUTHENTICATED_FULLY` (Bearer token) talab qilinadi.

## Loyihani o'rnatish va ishga tushirish bo'yicha buyruqlar

```bash
# 1. Yangi Symfony loyiha yaratish va paketlarni o'rnatish
symfony new user_project
cd user_project
composer require api
composer require --dev symfony/maker-bundle
composer require lexik/jwt-authentication-bundle

# 2. JWT kalitlarini generatsiya qilish
php bin/console lexik:jwt:generate-keypair

# 3. Ma'lumotlar bazasini yaratish va migratsiya qilish
php bin/console doctrine:database:create
php bin/console make:migration
php bin/console doctrine:migrations:migrate

# 4. Serverni ishga tushirish
symfony server:start
```

## GitHub'ga yuklash bo'yicha yo'riqnoma

```bash
git init
git add .
git commit -m "Feat: Symfony User API Platform project with Factory, Manager, Controller and JWT"
git branch -M main
git remote add origin https://github.com/<SIZNING_USERNAME>/<REPO_NOMI>.git
git push -u origin main
```
