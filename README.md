# my-mac

نسخه مک فلیت OpenCode — مثل `my-opencode` ولی روی `macos-latest`، بدون 9router.

- یک ماشین: `mac1` (`.github/workflows/mac1.yml`، شاخه `opc`)
- اجرا فقط دستی است: Actions → `mac1` → Run workflow. هیچ scheduleای نیست.
- بعد از بوت، از روی Tailscale در دسترس است: SSH (`runner`)، Laravel روی `:8000`، OpenCode روی `:4096` (کاربر `opencode`)، و ریموت دسکتاپ VNC روی `:5900` (فقط Tailscale).
- موقتی است مثل فلیت اوبونتو: سقف ۳۴۰ دقیقه، و state در پایان ران در مرحله ۱۵ به همین ریپو برمی‌گردد (`opencode/config` مشترک + `opencode/instances/mac1/`).

## Secrets (صاحب ریپو خودش می‌سازد — هیچ مقداری اینجا نیست)

Settings → Secrets and variables → Actions → New repository secret:

- `MY_PAT` — توکن گیت‌هاب صاحب ریپو
- `RUNNER_PASSWORD` — پسورد `runner` (هم SSH، هم OpenCode، هم VNC)
- `TAILSCALE_AUTHKEY` — auth key تیلسکیل

بدون این سه‌تا، ران بوت نمی‌شود.

## VNC — آزمایشی

مرحله ۱۱ Screen Sharing مک را با `kickstart` روشن می‌کند و پسورد legacy VNC را همان `RUNNER_PASSWORD` می‌گذارد. آدرس در لاگ همان مرحله چاپ می‌شود: `vnc://<Tailscale IP>:5900`.

- فقط از روی Tailscale وصل شو؛ پورت 5900 رمزنگاری ندارد.
- پسورد legacy مک حداکثر ۸ کاراکتر است؛ اگر پسورد کامل جواب نداد، ۸ کاراکتر اولش را امتحان کن.
- اگر مرحله ۱۱ در لاگ گفت چیزی روی `:5900` گوش نمی‌دهد یا `screencapture` تصویر نداد، VNC بالا نیامده؛ SSH و وب‌ها همچنان کار می‌کنند. این مرحله عمداً ران را fail نمی‌کند.

## تفاوت‌ها با فلیت اوبونتو

مبنای `mac1.yml` فایل زنده `syd1.yml` از `haileen5/my-opencode` است. تفاوت‌های عمدی: `macos-latest`؛ PHP با `shivammathur/setup-php`؛ PostgreSQL/PostGIS و Redis با Homebrew (بدون Docker)؛ Tailscale با Homebrew؛ `codebase-memory-mcp` نسخه `darwin-arm64`؛ مسیرها با `$HOME` (`/Users/runner`)؛ و هیچ اثری از 9router یا `wreq-js` نیست.
