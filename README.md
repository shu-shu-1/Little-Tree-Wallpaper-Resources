# Little Tree Wallpaper Resources

[中文说明](README_zh.md)

> 🌳 A metadata registry for plugins, themes, and wallpaper sources of **Little Tree Wallpaper**.

---

## 📌 Purpose

This repository **does NOT contain actual wallpapers, plugins, or themes**.  
It only provides **metadata files** (in TOML/JSON format) that point to third-party resources hosted elsewhere (e.g., on GitHub).

You can think of it as a "**store catalog**" or "**app store index**" for Little Tree Wallpaper.

---

## 📂 Structure

```text
little-tree-resources/
├── plugins/       # Plugin metadata (.toml)
├── themes/        # Theme metadata (.json or .toml)
├── resources/    # Wallpaper source metadata (with categories, preview URLs, etc.)
├── LICENSE        # License terms
└── README.md      # This file
```

Each file describes:
- Name, author, description  
- Version and update URL  
- Source repository (e.g., GitHub link)  
- License of the **linked resource** (if known)

---

## ⚖️ License

The **metadata files in this repository** are licensed under the:  
> **[Little Tree Wallpaper Resources License (LTR License)](./LICENSE)**

### Key Terms:
- ✅ **Attribution required**: You must credit “Little Tree Wallpaper Resources” when using this data.  
- ❌ **Non-commercial use only**: No use in paid products or commercial services.  
- 🚫 **Integration requires permission**: To use this data in your app or service, **contact Little Tree Studio for prior approval**.  
- 🛡️ **No warranty**: We do not guarantee the legality, copyright status, or safety of linked resources.

> For permission requests, please contact: **studio@zsxiaoshu.cn**

---

## ⚠️ Disclaimer

- This repository **only provides links and metadata**.  
- All wallpapers, plugins, and themes are **owned by their respective authors**.  
- We **do NOT verify** the license, copyright status, or content safety of linked resources.  
- **You are solely responsible** for complying with the licenses of the actual resources you use.

---

## 🤝 Contributing

Want to add your plugin, theme, or wallpaper source?

1. Fork this repository  
2. Add a `.toml` file under the appropriate directory (`plugins/`, `themes/`, or `wallpapers/`)  
3. Submit a Pull Request with a clear description  

Please ensure your resource:
- Is publicly accessible (e.g., on GitHub)  
- Includes a valid license  
- Does not contain illegal or infringing content  

---

## 🌐 Official Project

- Main Application: [Little Tree Wallpaper Next](https://github.com/shu-shu-1/Little-Tree-Wallpaper-Next-Flet)  
- Resources Repository: [Little Tree Resources](https://github.com/shu-shu-1/Little-Tree-Wallpaper-Resources/)  

For detailed contribution and metadata format guidelines, see our [Documentation Site](https://docs.zsxiaoshu.cn) (coming soon).

> 🌱 Built with care for a healthy wallpaper ecosystem.