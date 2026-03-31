# 2D Java Game Framework

Lightweight collection of Java classes to help build simple 2D games.
It provides a reusable `game.framework.Game` base class with a window + rendering loop, input polling (Keyboard/Mouse), timing (`GameTime`), and math/graphics helpers.

## Requirements

- JDK 7+ (project originally targeted JDK7)
- Java AWT/Swing (framework uses `JFrame`, `Canvas`, `BufferStrategy`)
- Build scripts:
  - Windows: `make.bat`
  - Linux/macOS: `make.sh`

## Build

1. Run `make.bat` (Windows) or `./make.sh` (Linux/macOS).
2. A `Game.jar` file will be generated in the repo root.

Luu y: `make.bat/make.sh` hiện tai chi `javac` cac package `game.debug`, `game.framework`, `game.graphics`, `game.input` truoc khi dong jar. Neu ban can du toan bo framework (vi du `game.animation`, `game.gui`, ...), hay build bang NetBeans/Ant tu `build.xml`.

Note: `Game.jar` is ignored by git (see `.gitignore`), so consumers should build it locally or copy the generated jar.

## Run the included demos

The default entrypoint configured for NetBeans/Ant is:

- `game.debug.TestFramework`

You can also run:

- `game.debug.Template` (minimal skeleton extending `game.framework.Game`)

## Luồng hoạt động của project

1. `game.debug.TestFramework.main()` tạo instance `TestFramework`, set title/kích thước rồi gọi `run()`.
2. `game.framework.Game.run()` làm các bước:
   - `createWindow()` (tạo `Canvas`, `BufferStrategy`, tạo `VolatileImage` off-screen và set size vào `GameHelper`)
   - `initialize()` (bật `running = true`, nền mặc định màu xanh dương, đăng listener Keyboard/Mouse, tạo `GameTime`)
   - `gameLoop()`
3. `gameLoop()` lặp khi còn `running`:
   - `gameTime.tick()`
   - `update(gameTime)` (lớp `Game` sẽ poll keyboard/mouse)
   - gọi `draw(Graphics2D)` để vẽ frame (thường bạn nên gọi `super.draw(g2d)` để clear nền)
   - blit từ `VolatileImage` sang màn hình bằng `BufferStrategy.show()`

Trong các demo, nhấn `ESC` để thoát (`Game.exitGame()`).

## Cách dùng như một framework

Tạo một class `extends game.framework.Game` và implement:

- `initialize()`
- `loadContent()`
- `unloadContent()`
- `update(GameTime gameTime)`
- `draw(Graphics2D g2d)`

Khuyến nghị:
- Gọi `super.initialize()` / `super.update(gameTime)` / `super.draw(g2d)` để giữ phần boilerplate của framework hoạt động đúng.
- Logic theo frame đặt trong `update`, vẽ đặt trong `draw`.
- Gọi `run()` để bắt đầu window + loop.

## Project structure

- `game.framework`: engine core (`Game`, `GameTime`, helpers)
- `game.input`: keyboard/mouse polling wrapper
- `game.graphics`: texture/render helpers
- `game.animation`: tweening + easing functions
- `game.gui`: GUI components/states
- `game.debug`: demo entrypoints

