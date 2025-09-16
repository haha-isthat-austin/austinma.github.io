# austinma.github.io

<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Austin Ma — Portfolio</title>

    <!-- Tailwind (CDN for quick preview) -->
    <script src="https://cdn.tailwindcss.com"></script>

    <!-- React 18 UMD + Babel (for JSX in-browser) -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  </head>
  <body class="min-h-screen bg-gradient-to-b from-white to-neutral-100 text-neutral-900">
    <div id="root"></div>

    <script type="text/babel">
      const { useEffect, useMemo, useRef, useState } = React;

      function ASCIIAnimationHeader({ frames, fps = 8, className = "" }) {
        const [idx, setIdx] = useState(0);
        const timerRef = useRef(null);
        const safeFrames = useMemo(() => (frames?.length ? frames : defaultAsciiFrames), [frames]);

        useEffect(() => {
          const interval = 1000 / Math.max(1, fps);
          timerRef.current = setInterval(() => setIdx(i => (i + 1) % safeFrames.length), interval);
          return () => clearInterval(timerRef.current);
        }, [safeFrames, fps]);

        return (
          <div className={"w-full overflow-hidden rounded-2xl shadow bg-black/90 text-green-300 border border-green-500/30 " + className}>
            <pre className="whitespace-pre leading-[0.95] p-4 overflow-auto text-xs sm:text-sm md:text-base lg:text-lg xl:text-xl font-mono select-none" aria-label="ASCII art animation" role="img">
{safeFrames[idx]}
            </pre>
          </div>
        );
      }

      const defaultAsciiFrames = [
` ~~~   ~~~   ~~~   ~~~   ~~~ 
|  \\   |  \\   |  \\   |  \\   |  \\ 
|   \\  |   \\  |   \\  |   \\  |   \\ 
|      
|     *  *  *  *  *   PORTFOLIO  *  *  *  *  *
|      
|   //  |   // |   // |   // |   //
|  //   |  //  |  //  |  //  |  //
 ~~~   ~~~   ~~~   ~~~   ~~~`,
` ~~~   ~~~   ~~~   ~~~   ~~~ 
|  //  |  //  |  //  |  //  |  //
| //   | //   | //   | //   | //
|/     
|     *  *  *  *  *   PORTFOLIO  *  *  *  *  *
|\\     
| \\    | \\    | \\    | \\    | \\ 
|  \\   |  \\   |  \\   |  \\   |  \\ 
 ~~~   ~~~   ~~~   ~~~   ~~~`,
      ];

      const posts = [
        { title: "Designing a Low-Noise ADC Front-End", date: "2025-08-30", tags: ["hardware","analog","adc"], excerpt: "Trade-offs between input impedance, anti-aliasing, and source load for a 24-bit ΔΣ front-end.", href: "#" },
        { title: "A Photographer’s Guide to Color Science", date: "2025-07-18", tags: ["photo","color","grading"], excerpt: "From sensor CFA to display EOTF: demystifying the pipe and getting reliable skin tones.", href: "#" },
        { title: "STM32 I2C: DMA Pitfalls & Patterns", date: "2025-06-11", tags: ["embedded","stm32","i2c"], excerpt: "Hard-won lessons using HAL + DMA for robust, low-jitter peripheral comms.", href: "#" },
        { title: "Bootstrapping a Hydroponic Street Farm", date: "2025-05-22", tags: ["farming","startup","hydroponics"], excerpt: "A tactical, numbers-first playbook.", href: "#" },
      ];

      function formatDate(iso) {
        const d = new Date(iso);
        return d.toLocaleDateString(undefined, { year: "numeric", month: "short", day: "2-digit" });
      }

      function SearchBar({ value, onChange }) {
        return (
          <input
            value={value}
            onChange={e => onChange(e.target.value)}
            placeholder="Search posts, tags…"
            className="w-full md:w-96 rounded-2xl border border-neutral-300 bg-white/70 backdrop-blur px-4 py-2 outline-none focus:ring-2 focus:ring-indigo-500"
          />
        );
      }

      function Tag({ children }) {
        return <span className="text-xs md:text-sm rounded-full border px-2 py-0.5 bg-neutral-100 border-neutral-300">{children}</span>;
      }

      function PortfolioSite() {
        const [query, setQuery] = useState("");
        const filtered = useMemo(() => {
          const q = query.trim().toLowerCase();
          if (!q) return posts;
          return posts.filter(p =>
            p.title.toLowerCase().includes(q) ||
            p.excerpt.toLowerCase().includes(q) ||
            p.tags.some(t => t.toLowerCase().includes(q))
          );
        }, [query]);

        // Generate custom ASCII title frames
        const frames = useMemo(() => makeAsciiTitleFrames("AUSTIN MA — PORTFOLIO"), []);

        return (
          <div className="min-h-screen">
            <nav className="sticky top-0 z-40 backdrop-blur bg-white/70 border-b border-neutral-200">
              <div className="max-w-6xl mx-auto flex items-center justify-between p-3">
                <a href="#home" className="font-semibold tracking-tight">am.dev</a>
                <div className="flex items-center gap-3">
                  <a href="#blog" className="text-sm opacity-80 hover:opacity-100">Blog</a>
                  <a href="#projects" className="text-sm opacity-80 hover:opacity-100">Projects</a>
                  <a href="#about" className="text-sm opacity-80 hover:opacity-100">About</a>
                </div>
              </div>
            </nav>

            <section id="home" className="max-w-6xl mx-auto px-4 md:px-6 py-8 md:py-12 lg:py-16">
              <div className="grid md:grid-cols-5 gap-6 items-stretch">
                <div className="md:col-span-3">
                  <ASCIIAnimationHeader frames={frames} />
                </div>
                <div className="md:col-span-2 flex flex-col justify-between gap-4">
                  <div>
                    <h1 className="text-2xl md:text-3xl font-semibold tracking-tight">Engineer • Media • Systems</h1>
                    <p className="mt-2 text-sm md:text-base opacity-80">
                      Ethical hardware, deep-tech stories, and living knowledge maps.
                    </p>
                  </div>
                  <div className="flex flex-col gap-2">
                    <SearchBar value={query} onChange={setQuery} />
                    <p className="text-xs opacity-70">Try tags: <span className="font-mono">hardware</span>, <span className="font-mono">photo</span>, <span className="font-mono">stm32</span></p>
                  </div>
                </div>
              </div>
            </section>

            <section id="blog" className="max-w-6xl mx-auto px-4 md:px-6 pb-6 md:pb-10">
              <div className="flex items-end justify-between mb-3">
                <h2 className="text-xl md:text-2xl font-semibold tracking-tight">Latest Writing</h2>
                <a href="#" className="text-sm opacity-80 hover:opacity-100">RSS →</a>
              </div>
              {filtered.length === 0 ? (
                <div className="border border-dashed rounded-2xl p-8 text-center opacity-70">No posts match “{query}”.</div>
              ) : (
                <div className="grid md:grid-cols-2 gap-4">
                  {filtered.map(p => (
                    <article key={p.title} className="rounded-2xl border border-neutral-300 p-4 hover:shadow-md transition">
                      <div className="text-xs opacity-60">{formatDate(p.date)}</div>
                      <h3 className="mt-1 text-lg font-semibold"><a href={p.href} className="hover:underline">{p.title}</a></h3>
                      <p className="mt-1 text-sm opacity-80">{p.excerpt}</p>
                      <div className="mt-3 flex gap-2 flex-wrap">{p.tags.map(t => <Tag key={t}>{t}</Tag>)}</div>
                    </article>
                  ))}
                </div>
              )}
            </section>

            <section id="projects" className="max-w-6xl mx-auto px-4 md:px-6 py-6 md:py-10">
              <h2 className="text-xl md:text-2xl font-semibold tracking-tight mb-3">Projects</h2>
              <div className="grid md:grid-cols-3 gap-4">
                {[
                  { name: "Hardware Sunday Stream", description: "Weekly live PCB design + teardowns", href: "#" },
                  { name: "Bits 2 Atoms", description: "Media & BD studio for deep-tech founders", href: "#" },
                  { name: "Vertical Strawberry Rig", description: "Urban hydroponic prototype", href: "#" },
                ].map(pr => (
                  <a key={pr.name} href={pr.href} className="rounded-2xl border border-neutral-300 p-4 hover:shadow-md transition block">
                    <div className="text-lg font-semibold">{pr.name}</div>
                    <p className="mt-1 text-sm opacity-80">{pr.description}</p>
                  </a>
                ))}
              </div>
            </section>

            <section id="about" className="max-w-6xl mx-auto px-4 md:px-6 py-6 md:py-12">
              <div className="grid md:grid-cols-3 gap-6 items-start">
                <div className="md:col-span-2">
                  <h2 className="text-xl md:text-2xl font-semibold tracking-tight">About</h2>
                  <p className="mt-2 text-sm md:text-base opacity-85">
                    I’m Austin, an electrical engineer + media nerd in LA. I design PCBs, build sensors,
                    and create content that demystifies deep tech.
                  </p>
                </div>
                <div className="md:col-span-1 rounded-2xl border border-neutral-300 p-4">
                  <h3 className="font-semibold">Contact</h3>
                  <p className="text-sm opacity-85 mt-1">Email: hello@yourdomain.com<br/>Social: @austin_ma</p>
                </div>
              </div>
            </section>

            <footer className="border-t border-neutral-200 py-6">
              <div className="max-w-6xl mx-auto px-4 md:px-6 text-sm opacity-70 flex items-center justify-between">
                <span>© ${new Date().getFullYear()} Austin Ma</span>
                <div className="flex gap-3">
                  <a href="#" className="hover:underline">Privacy</a>
                  <a href="#" className="hover:underline">Imprint</a>
                </div>
              </div>
            </footer>
          </div>
        );
      }

      function makeAsciiTitleFrames(text) {
        const width = Math.max(48, text.length + 4);
        const pad = (s, w) => {
          const left = Math.floor((w - s.length) / 2);
          const right = w - s.length - left;
          return " ".repeat(left) + s + " ".repeat(right);
        };
        const symbols = ["~","-","=","~","-"];
        const frames = [];
        for (let f = 0; f < 12; f++) {
          const bannerTop = Array.from({ length: Math.ceil(width / 2) }, (_, i) => (i % 2 ? symbols[(f)%symbols.length] : " ")).join("");
          const bannerBot = Array.from({ length: Math.ceil(width / 2) }, (_, i) => (i % 2 ? symbols[(f+1)%symbols.length] : " ")).join("");
          const title = pad(text, width);
          const subtitle = pad("// engineer • media • systems", width);
          const border = ch => ch.repeat(width);
          frames.push(
            " " + bannerTop.slice(0, width) + "\n" +
            border(".") + "\n" +
            title + "\n" +
            subtitle + "\n" +
            border(".") + "\n" +
            " " + bannerBot.slice(0, width)
          );
        }
        return frames;
      }

      ReactDOM.createRoot(document.getElementById("root")).render(<PortfolioSite />);
    </script>
  </body>
</html>

