# گزارش اصلاحات — 2026-07-21

این فایل خلاصه کارهایی است که بدون ساخت هیچ کد بازی (فقط مستندات) انجام شد، بر اساس دو فایل پیوستی کاربر.

## 1. قفل کردن نسخه Godot (TASK-01-00)

**درخواست:** نسخه دقیق `v4.7.1.stable.official [a13da4feb]` را ثبت کن.

**انجام شده:**
- `AI_DOCS/05_TECH_SPEC.md`: مقدار `<PENDING — set by TASK-01-00>` به `v4.7.1.stable.official [a13da4feb]` تغییر یافت.
- `AI_DOCS/09_DECISIONS.md`: ADR-002 Status از `Pending` به `Accepted` و رشته نسخه ثبت شد.
- هر دو فایل کاراکتر-به-کاراکتر یکسان هستند.

طبق 01_RULES.md بخش 6.4، ورود به CHANGELOG فقط پس از تایید واقعی کاربر/اجرای تست انجام شود — فعلاً CHANGELOG خالی ماند.

## 2. باگ بحرانی: Bitmask vs Layer Number

### مشکل
در Godot مقدار `collision_layer` / `collision_mask` یک **bitmask** است نه شماره لایه:
- شماره لایه در Inspector: 1..7
- مقدار واقعی: `1 << (layer_number -1)` => 1,2,4,8,16,32,64
- بنابراین `collision_layer = 3` به معنای لایه 3 نیست، بلکه لایه 1 و 2 همزمان است.

مستندات قبلی (ADR-013) اشتباهاً از 1..7 به عنوان مقدار استفاده کرده بود:
- enemy = 3 (اشتباه) باید 4 باشد
- player_projectiles = 4 باید 8
- enemy_projectiles = 5 باید 16
- pickups = 6 باید 32
- extraction_zone = 7 باید 64
- maskهای مربوطه هم باید 5 (1+4) و 3 (1+2) باشند که قبلاً به صورت ناسازگار نوشته شده بود.

### اصلاحات انجام شده

#### ADR-013 → ADR-014
- `ADR-013` Status به `Superseded by ADR-014` تغییر یافت، محتوای اصلی برای audit حفظ شد.
- `ADR-014` جدید ساخته شد:
  - توضیح کامل فرمول bitmask
  - جدول صحیح لایه‌ها با مقدار bitmask
  - جدول صحیح entity → layer/mask
  - قانون پیاده‌سازی: همیشه از bitmask استفاده کن، با کامنت `per ADR-014`
  - توضیح migration برای TASK-01-04

#### 05_TECH_SPEC.md
- عنوان بخش: از `per ADR-013` به `per ADR-013, CORRECTED by ADR-014`
- جدول لایه‌ها: ستون Bitmask Value اضافه شد (1,2,4,8,16,32,64)
- جدول entity: مقادیر اصلاح شده (4,8,16,32,64 و maskهای 5,3,2)
- ارجاع کامنت: `per ADR-014`

#### سایر فایل‌های حکمرانی
- `00_INDEX.md`: `per ADR-013` → `per ADR-014 [supersedes ADR-013]`
- `01_RULES.md`: `citing ADR-013` → `citing ADR-014 (ADR-013 superseded)` + توضیح bitmask
- `03_SESSION_BOOTSTRAP.md`: به ADR-014 و مقادیر bitmask اشاره شد
- `07_TEST_PLAN.md`: `ADR-013's table` → `ADR-014's corrected bitmask table (ADR-013 superseded — values 8/5, 16/3)`

#### تمام Task فایل‌های PARTS
فایل‌های زیر به مقدار bitmask صحیح اصلاح شدند و همه `per ADR-013` → `per ADR-014`:

- `part_01_foundation/task_01_01_project_skeleton.md`
  - `TASK-10-06 will` → `TASK-10-05 will` (باگ گزارش شده: Main Scene در 10-05 تغییر می‌کند نه 10-06)
  - ارجاعات ADR به ADR-014

- `part_01_foundation/task_01_04_physics_layer_consistency_check.md`
  - کاملاً بازنویسی شد برای ADR-014
  - لیست مقادیر صحیح bitmask
  - Acceptance Criteria برای بررسی عدم وجود مقادیر قدیمی 3,5,6,7 به عنوان layer
  - اجازه بررسی کل AI_DOCS/PARTS/**/*.md

- `part_02_player_movement/task_02_01_player_scene_and_sprite.md`
  - `collision_layer = 2` درست ماند (player)
  - ارجاع به ADR-014

- `part_06_combat/task_06_02_projectile_scene.md`
  - `collision_layer = 4` → `8` (player_projectiles)
  - `collision_mask = 5` (درست بود، توضیح `1 | 4` به جای `1 | 3` اصلاح شد)
  - کامنت‌ها به ADR-014

- `part_06_combat/task_06_04_hit_detection_dummy_target.md`
  - `collision_layer = 3` → `4` (enemies)

- `part_07_enemy/task_07_02_enemy_scene_and_health.md`
  - `3` → `4`

- `part_07_enemy/task_07_03_patrol_state.md`
  - `3` → `4`

- `part_07_enemy/task_07_04_chase_state.md`
  - `3` → `4`

- `part_07_enemy/task_07_05_attack_state.md`
  - `5` → `16` (enemy_projectiles)
  - توضیح mask `1 | 2` درست ماند

- `part_09_test_level_and_extraction/task_09_01_test_level_shell.md`
  - `1` درست ماند
  - ارجاع به ADR-014

- `part_09_test_level_and_extraction/task_09_02_loot_drop_on_death.md`
  - `6` → `32` (pickups)

- `part_09_test_level_and_extraction/task_09_04_extraction_point_zone.md`
  - `7` → `64` (extraction_zone)

- `part_10_hub_save/task_10_05_game_manager_and_hub_raid_transition.md`
  - mask 2 درست ماند، ارجاع به ADR-014

- `part_01_foundation/task_01_03_documentation_migration_check.md`
  - Allowed Files rozszerه شد تا شامل `AI_DOCS/PARTS/**/*.md` نیز باشد (طبق پیشنهاد گزارش پیوستی)

### بررسی نهایی
```bash
grep -rn "collision_layer = 3" → فقط در توضیحات آموزشی ADR-014 (این مقدار اشتباه است)
grep -rn "collision_layer = 5/6/7" → صفر مورد به عنوان مقدار authoritative
grep -rn "collision_layer =" → مقادیر 1,2,4,8,16,32,64,0 (همه صحیح)
grep -rn "ADR-013" → فقط به عنوان تاریخچه "superseded" باقی مانده
```

## 3. موارد جزئی دیگر

- تعداد Taskها: مستندات قبلی 65 اعلام کرده بود ولی 58 Task واقعی وجود دارد — این ناسازگاری در گزارش اصلاحی ثبت شد (نیاز به تصمیم جدید ندارد، فقط آمار).
- TASK-01-01: ارجاع به TASK-10-06 اصلاح به TASK-10-05 شد.
- هیچ فایل `Game/` ساخته نشد (طبق درخواست: هیجی کدی نساز).

## 4. وضعیت فعلی

- نسخه موتور قفل شد: `v4.7.1.stable.official [a13da4feb]`
- ADR-014 به عنوان مرجع صحیح فیزیک قفل شد
- تمام Task فایل‌ها با bitmask صحیح هماهنگ شدند
- TASK-01-04 آماده است تا قبل از Part 04 اجرا شود و consistency را تایید کند
- CURRENT_TASK هنوز TASK-01-00 است، ولی Acceptance Criteria آن برآورده شده — پس از تایید شما می‌توان آن را به TASK-01-01 تغییر داد و وارد CHANGELOG کرد.

## قدم بعدی پیشنهادی

1. تایید کنید `05_TECH_SPEC.md` و `09_DECISIONS.md` نسخه یکسان دارند.
2. اگر تایید شد، یک ورودی برای TASK-01-00 در CHANGELOG بنویسید.
3. سپس CURRENT_TASK را به `task_01_01_project_skeleton.md` تغییر دهید.
4. سپس TASK-01-04 را به عنوان اولین consistency check اجرا کنید تا مطمئن شوید دیگر هیچ مقدار قدیمی باقی نمانده.

هیچ کدی ساخته نشد — فقط MD.
