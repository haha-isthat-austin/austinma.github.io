# austinma.github.io

import React, { useEffect, useMemo, useRef, useState } from "react";

// ===============================
// ASCII Header Animation Component
// ===============================
function ASCIIAnimationHeader({ frames, fps = 6, className = "" }) {
  const [idx, setIdx] = useState(0);
  const timerRef = useRef(null);

  const safeFrames = useMemo(() => (frames && frames.length ? frames : defaultAsciiFrames), [frames]);

  useEffect(() => {
    const interval = 1000 / Math.max(1, fps);
    timerRef.current = setInterval(() => setIdx((i) => (i + 1) % safeFrames.length), interval);
    return () => clearInterval(timerRef.current);
  }, [safeFrames, fps]);

  return (
    <div className={`w-full overflow-hidden rounded-2xl shadow-lg bg-black/90 text-green-300 border border-green-500/30 ${className}`}>
      <pre className="whitespace-pre leading-[0.95] p-4 overflow-auto text-xs sm:text-sm md:text-base lg:text-lg xl:text-xl font-mono select-none" aria-label="ASCII art animation" role="img">
        {safeFrames[idx]}
      </pre>
    </div>
  );
}

// Simple wavy banner frames (monospaced). Feel free to swap in your own.
const defaultAsciiFrames = [
` ~~~   ~~~   ~~~   ~~~   ~~~ \n|  \\  |  \\  |  \\  |  \\  |  \\ \n|   \\ |   \\ |   \\ |   \\ |   \\ \n|    \\\n|     *  *  *  *  *   PORTFOLIO  *  *  *  *  *\n|    //\n|   // |   // |   // |   // |   //\n|  //  |  //  |  //  |  //  |  //\n ~~~   ~~~   ~~~   ~~~   ~~~`,
` ~~~   ~~~   ~~~   ~~~   ~~~ \n|  //  |  //  |  //  |  //  |  //\n| //   | //   | //   | //   | //\n|/     \n|     *  *  *  *  *   PORTFOLIO  *  *  *  *  *\n|\\     \n| \\   | \\   | \\   | \\   | \\ \n|  \\  |  \\  |  \\  |  \\  |  \\ \n ~~~   ~~~   ~~~   ~~~   ~~~`,
];

// ================
// Mock Site Data
// ================
const posts = [
  {
    title: "Designing a Low-Noise ADC Front-End",
    date: "2025-08-30",
    tags: ["hardware", "analog", "adc"],
    excerpt: "Trade-offs between input impedance, anti-aliasing, and source load for a 24-bit ΔΣ front-end.",
    href: "#",
  },
  {
    title: "A Photographer’s Guide to Color Science",
    date: "2025-07-18",
    tags: ["photo", "color", "grading"],
    excerpt: "From sensor CFA to display EOTF: demystifying the pipe and getting reliable skin tones.",
    href: "#",
  },
  {
    title: "STM32 I2C: DMA Pitfalls & Patterns",
    date: "2025-06-11",
    tags: ["embedded", "stm32", "i2c"],
    excerpt: "Hard-won lessons using HAL + DMA for robust, low-jitter peripheral comms.",
    href: "#",
  },
  {
    title: "Bootstrapping a Hydroponic Street Farm",
    date: "2025-05-22",
    tags: ["farming", "startup", "hydroponics"],
    excerpt: "From nutrient film technique to city permits: a tactical, numbers-first playbook.",
    href: "#",
  },
];

const projects = [
  {
    name: "Hardware Sunday Stream",
    description: "Weekly live show: PCB design, teardown sessions, and audience Q&A.",
    href: "#",
  },
  {
    name: "Bits 2 Atoms",
    description: "Media & BD studio helping deep-tech founders tell their story.",
    href: "#",
  },
  {
    name: "Vertical Strawberry Rig",
    description: "An urban hydroponic prototype focused on energy & labor efficiency.",
    href: "#",
  },
];

// =====================
// Small Utility Pieces
// =====================
function classNames(...a) { return a.filter(Boolean).join(" "); }

function useDarkMode() {
  const [enabled, setEnabled] = useState(true);
  useEffect(() => {
    const root = document.documentElement;
    if (enabled) root.classList.add("dark"); else root.classList.remove("dark");
  }, [enabled]);
  return [enabled, setEnabled];
}

function SearchBar({ value, onChange }) {
  return (
    <input
      value={value}
      onChange={(e) => onChange(e.target.value)}
      placeholder="Search posts, tags…"
      className="w-full md:w-96 rounded-2xl border border-neutral-300 dark:border-neutral-700 bg-white/70 dark:bg-neutral-900/70 backdrop-blur px-4 py-2 outline-none focus:ring-2 focus:ring-indigo-500"
    />
  );
}

function Tag({ children }) {
  return (
    <span className="text-xs md:text-sm rounded-full border px-2 py-0.5 bg-neutral-100 dark:bg-neutral-800 border-neutral-300 dark:border-neutral-700">{children}</span>
  );
}

// ==============
// Main Component
// ==============
export default function PortfolioSite() {
  const [query, setQuery] = useState("");
  const [dark, setDark] = useDarkMode();

  const filtered = useMemo(() => {
    const q = query.trim().toLowerCase();
    if (!q) return posts;
    return posts.filter((p) =>
      p.title.toLowerCase().includes(q) ||
      p.excerpt.toLowerCase().includes(q) ||
      p.tags.some((t) => t.toLowerCase().includes(q))
    );
  }, [query]);

  const asciiFrames = useMemo(() => makeAsciiTitleFrames("AUSTIN MA — PORTFOLIO"), []);

  return (
    <div className="min-h-screen bg-gradient-to-b from-white to-neutral-100 dark:from-neutral-950 dark:to-neutral-900 text-neutral-900 dark:text-neutral-100">
      {/* NAVBAR */}
      <nav className="sticky top-0 z-40 backdrop-blur bg-white/60 dark:bg-neutral-950/60 border-b border-neutral-200 dark:border-neutral-800">
        <div className="max-w-6xl mx-auto flex items-center justify-between p-3">
          <a href="#home" className="font-semibold tracking-tight">am.dev</a>
          <div className="flex items-center gap-3">
            <a href="#blog" className="text-sm opacity-80 hover:opacity-100">Blog</a>
            <a href="#projects" className="text-sm opacity-80 hover:opacity-100">Projects</a>
            <a href="#about" className="text-sm opacity-80 hover:opacity-100">About</a>
            <button onClick={() => setDark(!dark)} className="ml-2 text-xs px-3 py-1.5 rounded-full border border-neutral-300 dark:border-neutral-700 hover:bg-neutral-100 dark:hover:bg-neutral-800">
              {dark ? "Light" : "Dark"}
            </button>
          </div>
        </div>
      </nav>

      {/* HERO */}
      <section id="home" className="max-w-6xl mx-auto px-4 md:px-6 py-8 md:py-12 lg:py-16">
        <div className="grid md:grid-cols-5 gap-6 items-stretch">
          <div className="md:col-span-3">
            <ASCIIAnimationHeader frames={asciiFrames} fps={8} />
          </div>
          <div className="md:col-span-2 flex flex-col justify-between gap-4">
            <div>
              <h1 className="text-2xl md:text-3xl font-semibold tracking-tight">Engineer • Media • Systems</h1>
              <p className="mt-2 text-sm md:text-base opacity-80">I build ethical hardware, tell deep‑tech stories, and map knowledge so others can stand on our shoulders. This is a living portfolio: essays, projects, and experiments.</p>
            </div>
            <div className="flex flex-col gap-2">
              <SearchBar value={query} onChange={setQuery} />
              <p className="text-xs opacity-70">Tip: try tags like <span className="font-mono">hardware</span>, <span className="font-mono">photo</span>, <span className="font-mono">stm32</span></p>
            </div>
          </div>
        </div>
      </section>

      {/* BLOG LIST */}
      <section id="blog" className="max-w-6xl mx-auto px-4 md:px-6 pb-6 md:pb-10">
        <div className="flex items-end justify-between mb-3">
          <h2 className="text-xl md:text-2xl font-semibold tracking-tight">Latest Writing</h2>
          <a href="#" className="text-sm opacity-80 hover:opacity-100">RSS →</a>
        </div>
        {filtered.length === 0 ? (
          <div className="border border-dashed rounded-2xl p-8 text-center opacity-70">No posts match “{query}”.</div>
        ) : (
          <div className="grid md:grid-cols-2 gap-4">
            {filtered.map((p) => (
              <article key={p.title} className="rounded-2xl border border-neutral-300 dark:border-neutral-800 p-4 hover:shadow-md transition">
                <div className="text-xs opacity-60">{formatDate(p.date)}</div>
                <h3 className="mt-1 text-lg font-semibold"><a href={p.href} className="hover:underline">{p.title}</a></h3>
                <p className="mt-1 text-sm opacity-80">{p.excerpt}</p>
                <div className="mt-3 flex gap-2 flex-wrap">{p.tags.map((t) => <Tag key={t}>{t}</Tag>)}</div>
              </article>
            ))}
          </div>
        )}
      </section>

      {/* PROJECTS */}
      <section id="projects" className="max-w-6xl mx-auto px-4 md:px-6 py-6 md:py-10">
        <h2 className="text-xl md:text-2xl font-semibold tracking-tight mb-3">Projects</h2>
        <div className="grid md:grid-cols-3 gap-4">
          {projects.map((pr) => (
            <a key={pr.name} href={pr.href} className="rounded-2xl border border-neutral-300 dark:border-neutral-800 p-4 hover:shadow-md transition block">
              <div className="text-lg font-semibold">{pr.name}</div>
              <p className="mt-1 text-sm opacity-80">{pr.description}</p>
            </a>
          ))}
        </div>
      </section>

      {/* ABOUT */}
      <section id="about" className="max-w-6xl mx-auto px-4 md:px-6 py-6 md:py-12">
        <div className="grid md:grid-cols-3 gap-6 items-start">
          <div className="md:col-span-2">
            <h2 className="text-xl md:text-2xl font-semibold tracking-tight">About</h2>
            <p className="mt-2 text-sm md:text-base opacity-85">I’m Austin, an electrical engineer + media nerd based in Los Angeles. I design PCBs, build sensors, and create content that demystifies deep tech. When I’m not routing, you’ll find me shooting or grading footage, or experimenting with urban hydroponics and public‑goods tooling.</p>
            <ul className="mt-4 text-sm list-disc list-inside opacity-85 space-y-1">
              <li>Currently exploring: ASIC toy labs, knowledge graphs, and ethical hardware stacks.</li>
              <li>Interests: mmWave radar, color pipelines, city farming, and systems thinking.</li>
            </ul>
          </div>
          <div className="md:col-span-1 rounded-2xl border border-neutral-300 dark:border-neutral-800 p-4">
            <h3 className="font-semibold">Contact</h3>
            <p className="text-sm opacity-85 mt-1">Email: hello@yourdomain.com<br/>Social: @austin_ma</p>
            <div className="mt-3 flex gap-2">
              <a href="#" className="text-xs px-3 py-1.5 rounded-full border border-neutral-300 dark:border-neutral-700 hover:bg-neutral-100 dark:hover:bg-neutral-800">Resume</a>
              <a href="#" className="text-xs px-3 py-1.5 rounded-full border border-neutral-300 dark:border-neutral-700 hover:bg-neutral-100 dark:hover:bg-neutral-800">Media Kit</a>
            </div>
          </div>
        </div>
      </section>

      {/* FOOTER */}
      <footer className="border-t border-neutral-200 dark:border-neutral-800 py-6">
        <div className="max-w-6xl mx-auto px-4 md:px-6 text-sm opacity-70 flex items-center justify-between">
          <span>© {new Date().getFullYear()} Austin Ma</span>
          <div className="flex gap-3">
            <a href="#" className="hover:underline">Privacy</a>
            <a href="#" className="hover:underline">Imprint</a>
          </div>
        </div>
      </footer>
    </div>
  );
}

// =====================
// Helper functions
// =====================
function formatDate(iso) {
  const d = new Date(iso);
  return d.toLocaleDateString(undefined, { year: "numeric", month: "short", day: "2-digit" });
}

// Generates simple oscillating frames for a big ASCII title.
function makeAsciiTitleFrames(text) {
  const width = Math.max(48, text.length + 4);
  const pad = (s, w) => {
    const left = Math.floor((w - s.length) / 2);
    const right = w - s.length - left;
    return " ".repeat(left) + s + " ".repeat(right);
  };
  const wave = (row, phase) => {
    // row is 0..N-1; we modulate border decorations a bit
    const t = phase + row;
    const symbols = ["~", "-", "=", "~", "-"]; // cycles
    return symbols[(t % symbols.length + symbols.length) % symbols.length];
  };

  const frames = [];
  for (let f = 0; f < 12; f++) {
    const bannerTop = Array.from({ length: Math.ceil(width / 2) }, (_, i) => (i % 2 ? wave(0, f) : " ")).join("");
    const bannerBot = Array.from({ length: Math.ceil(width / 2) }, (_, i) => (i % 2 ? wave(1, f + 1) : " ")).join("");
    const title = pad(text, width);
    const subtitle = pad("// engineer • media • systems", width);
    const border = (ch) => ch.repeat(width);

    const frame = [
      " " + bannerTop.slice(0, width),
      border("."),
      title,
      subtitle,
      border("."),
      " " + bannerBot.slice(0, width),
    ].join("\n");
    frames.push(frame);
  }
  return frames;
}
