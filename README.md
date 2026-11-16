# glisse skill for Claude Code

A Claude Code skill for [glisse](https://glisse.kelvinzhang.ca), the React library that brings Apple's Liquid Glass to the web.

With this skill installed, Claude knows glisse's components, props, theming tokens and the browser gotchas (Safari/iOS refraction, React Router links). It loads on its own when you ask for glass or iOS-style UI. You don't have to name the library.

## Install

**Option 1: plugin (recommended, updates in one command)**

In Claude Code:

```
/plugin marketplace add OWNER/glisse-skill
/plugin install glisse@glisse-skill
```

**Option 2: skills CLI**

```bash
npx skills add OWNER/glisse-skill
```

**Option 3: copy by hand**

```bash
git clone https://github.com/OWNER/glisse-skill
# for all your projects:
cp -r glisse-skill/skills/glisse ~/.claude/skills/glisse
# or for one project only:
cp -r glisse-skill/skills/glisse <project>/.claude/skills/glisse
```

Restart Claude Code after installing.

## Use

Just ask:

- "Add a liquid glass tab bar to the bottom of the app"
- "Make the settings toggles iOS style"
- "Put the balance card on glass over the header image"

You can also call it explicitly with `/glisse`.

The skill installs `glisse` into your app (`npm install glisse`) if it isn't there yet.

## What's inside

```
skills/glisse/
  SKILL.md            # when to use it, setup, rules, theming
  references/api.md   # every component's props, with examples
```

## Uzbekcha

Bu Claude Code uchun skill. U [glisse](https://glisse.kelvinzhang.ca) (React uchun Apple Liquid Glass) kutubxonasini qanday ishlatishni biladi. O'rnatgandan keyin "Glisse'dan foydalanib..." deb yozish shart emas. "Liquid glass", "shisha effekt", "iOS uslubidagi tugma/tab bar" kabi so'rovlarda skill o'zi yuklanadi. Aniq chaqirish uchun `/glisse` yozing.

O'rnatish: Claude Code ichida `/plugin marketplace add OWNER/glisse-skill`, keyin `/plugin install glisse@glisse-skill`.

## License

MIT. glisse itself is MIT-licensed by Kelvin Zhang. This repo only contains documentation for using it.
