# Baby Name Swiper 👶💙💗

A fun, Tinder-style swipe interface for discovering and saving baby names.

![Status](https://img.shields.io/badge/status-in%20development-yellow)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## 🎯 What It Does

Swipe through hundreds of baby names with a simple, intuitive interface:
- **👉 Swipe right** to like a name
- **👈 Swipe left** to pass
- **❤️ View favorites** to review your liked names
- **💾 Auto-save** your progress locally

Perfect for expectant parents overwhelmed by endless name lists!

---

## 🚀 Quick Start

**Prerequisites:** Node.js 18+

```bash
# Clone and navigate
git clone <your-repo-url>
cd baby-name-swiper

# Follow setup guide
cat QUICKSTART.md
```

See **[QUICKSTART.md](QUICKSTART.md)** for complete setup instructions.

---

## 📋 Features

### Current POC
- ✅ Swipe-based name browsing
- ✅ Like/dismiss names with gestures or buttons
- ✅ Favorites collection
- ✅ Local storage persistence
- ✅ Mobile responsive design
- ✅ Progress tracking

### Future Enhancements
- [ ] Partner mode (compare favorites with partner)
- [ ] Advanced filters (by origin, length, starting letter)
- [ ] Undo last swipe
- [ ] Export favorites to PDF
- [ ] Name pronunciation audio
- [ ] Share favorites via link

---

## 🛠️ Tech Stack

- **Framework:** Svelte 4
- **Build Tool:** Vite
- **Styling:** Tailwind CSS
- **Storage:** Browser localStorage
- **Deployment:** Azure Static Web Apps (planned)

---

## 📁 Project Structure

```
src/
├── lib/
│   ├── components/     # Svelte UI components
│   ├── stores/         # State management
│   └── data/           # Name dataset JSON
├── app.css             # Tailwind imports
├── App.svelte          # Root component
└── main.js             # Entry point
```

---

## 📖 Documentation

- **[PRD.md](PRD.md)** - Product requirements & implementation checklist
- **[QUICKSTART.md](QUICKSTART.md)** - Step-by-step setup guide
- **[.github/copilot-instructions.md](.github/copilot-instructions.md)** - AI coding agent guidelines

---

## 🧪 Development

```bash
# Start dev server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

---

## 📊 Name Dataset

Sample dataset included: **25 popular names**

To expand:
1. Edit `src/lib/data/names.json`
2. Use sample format from `sample-names.json`
3. Sources: [SSA](https://www.ssa.gov/oact/babynames/), [Behind the Name](https://www.behindthename.com/)

---

## 🚢 Deployment

### Azure Static Web Apps (Planned)
1. Build: `npm run build`
2. Deploy `dist/` folder to Azure
3. Configure custom domain in Azure portal

---

## 🤝 Contributing

This is a personal project, but suggestions welcome!

1. Fork the repo
2. Create feature branch
3. Make your changes
4. Submit pull request

---

## 📝 License

MIT License - feel free to use for your own baby name journey!

---

## 🎨 Design Inspiration

- **Tinder** - Card swipe mechanics
- **Duolingo** - Simple, focused UI
- **Headspace** - Playful animations

---

## 💡 Tips for Use

- **Start with filters?** No! Just swipe - you'll discover names you wouldn't have searched for
- **How many to swipe?** Go through at least 50-100 before reviewing favorites
- **Partner sharing?** Both swipe independently, then compare favorites lists
- **Too many likes?** Be more selective on second pass through favorites

---

**Happy name hunting!** 🍼✨
