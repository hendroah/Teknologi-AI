import { useState, useEffect, useRef } from "react";

const articles = {
  title: "Era Baru Kecerdasan Buatan",
  subtitle: "Bagaimana AI Mengubah Dunia di Tahun 2026",
};

const adoptionData = [
  { year: "2021", persen: 35 },
  { year: "2022", persen: 50 },
  { year: "2023", persen: 62 },
  { year: "2024", persen: 72 },
  { year: "2025", persen: 78 },
  { year: "2026*", persen: 88 },
];

const industriData = [
  { industri: "Kesehatan", dampak: 92, warna: "#00E5C3" },
  { industri: "Manufaktur", dampak: 85, warna: "#00B4FF" },
  { industri: "Keuangan", dampak: 88, warna: "#7C5CFF" },
  { industri: "Ritel", dampak: 74, warna: "#FF6B6B" },
  { industri: "Pendidikan", dampak: 67, warna: "#FFB347" },
  { industri: "Hukum", dampak: 71, warna: "#90EE90" },
];

const trendData = [
  {
    no: "01",
    tren: "Agentic AI",
    deskripsi: "AI otonom yang merencanakan & mengeksekusi tugas tanpa instruksi manual",
    status: "Berkembang Pesat",
    dampak: "Sangat Tinggi",
    statusColor: "#00E5C3",
  },
  {
    no: "02",
    tren: "AI Multimodal",
    deskripsi: "Memproses teks, gambar, audio & video sekaligus dalam satu platform",
    status: "Sudah Aktif",
    dampak: "Tinggi",
    statusColor: "#00B4FF",
  },
  {
    no: "03",
    tren: "AI Vertikal",
    deskripsi: "Model AI khusus bidang: kesehatan, hukum, keuangan — lebih akurat",
    status: "Berkembang",
    dampak: "Tinggi",
    statusColor: "#7C5CFF",
  },
  {
    no: "04",
    tren: "Komputasi Kuantum",
    deskripsi: "Beralih dari lab ke sistem hibrida komersial skala nyata",
    status: "Awal Adopsi",
    dampak: "Masa Depan",
    statusColor: "#FFB347",
  },
  {
    no: "05",
    tren: "AI Edge",
    deskripsi: "Pemrosesan AI langsung di perangkat tanpa ketergantungan cloud",
    status: "Sudah Aktif",
    dampak: "Tinggi",
    statusColor: "#FF6B6B",
  },
  {
    no: "06",
    tren: "Zero Trust AI",
    deskripsi: "Keamanan siber berbasis AI: verifikasi setiap akses tanpa pengecualian",
    status: "Kritis",
    dampak: "Sangat Tinggi",
    statusColor: "#FF4444",
  },
];

const investasiData = [
  { wilayah: "Amerika Serikat", nilai: 1.4, emoji: "🇺🇸" },
  { wilayah: "Tiongkok", nilai: 0.85, emoji: "🇨🇳" },
  { wilayah: "Eropa", nilai: 0.62, emoji: "🇪🇺" },
  { wilayah: "Asia Pasifik", nilai: 0.38, emoji: "🌏" },
  { wilayah: "Lainnya", nilai: 0.15, emoji: "🌍" },
];

const totalInvestasi = investasiData.reduce((a, b) => a + b.nilai, 0);

function AnimatedNumber({ target, suffix = "", decimals = 0 }) {
  const [val, setVal] = useState(0);
  const ref = useRef(null);

  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          let start = 0;
          const duration = 1400;
          const step = (timestamp) => {
            if (!start) start = timestamp;
            const progress = Math.min((timestamp - start) / duration, 1);
            const ease = 1 - Math.pow(1 - progress, 3);
            setVal(parseFloat((ease * target).toFixed(decimals)));
            if (progress < 1) requestAnimationFrame(step);
          };
          requestAnimationFrame(step);
          observer.disconnect();
        }
      },
      { threshold: 0.3 }
    );
    if (ref.current) observer.observe(ref.current);
    return () => observer.disconnect();
  }, [target, decimals]);

  return <span ref={ref}>{decimals > 0 ? val.toFixed(decimals) : Math.round(val)}{suffix}</span>;
}

function BarChart() {
  const [visible, setVisible] = useState(false);
  const ref = useRef(null);
  const max = Math.max(...adoptionData.map((d) => d.persen));

  useEffect(() => {
    const observer = new IntersectionObserver(
      ([e]) => { if (e.isIntersecting) { setVisible(true); observer.disconnect(); } },
      { threshold: 0.3 }
    );
    if (ref.current) observer.observe(ref.current);
    return () => observer.disconnect();
  }, []);

  return (
    <div ref={ref} style={{ width: "100%", padding: "0 0 8px 0" }}>
      <div style={{ display: "flex", alignItems: "flex-end", gap: "10px", height: "160px", marginBottom: "8px" }}>
        {adoptionData.map((d, i) => (
          <div key={d.year} style={{ flex: 1, display: "flex", flexDirection: "column", alignItems: "center", height: "100%", justifyContent: "flex-end" }}>
            <div style={{
              fontSize: "11px", fontWeight: "700", color: "#00E5C3",
              marginBottom: "4px", opacity: visible ? 1 : 0,
              transition: `opacity 0.4s ${0.1 * i + 0.6}s`
            }}>{d.persen}%</div>
            <div style={{
              width: "100%", maxWidth: "44px",
              height: visible ? `${(d.persen / max) * 130}px` : "0px",
              background: i === adoptionData.length - 1
                ? "linear-gradient(180deg, #00E5C3, #00B4FF)"
                : "linear-gradient(180deg, #00B4FF88, #7C5CFF88)",
              borderRadius: "6px 6px 2px 2px",
              transition: `height 0.7s cubic-bezier(0.34,1.56,0.64,1) ${0.1 * i}s`,
              border: i === adoptionData.length - 1 ? "1px solid #00E5C3" : "1px solid #00B4FF44",
            }} />
          </div>
        ))}
      </div>
      <div style={{ display: "flex", gap: "10px" }}>
        {adoptionData.map((d) => (
          <div key={d.year} style={{ flex: 1, textAlign: "center", fontSize: "10px", color: "#aaa", fontFamily: "monospace" }}>{d.year}</div>
        ))}
      </div>
    </div>
  );
}

function HorizontalBars() {
  const [visible, setVisible] = useState(false);
  const ref = useRef(null);

  useEffect(() => {
    const observer = new IntersectionObserver(
      ([e]) => { if (e.isIntersecting) { setVisible(true); observer.disconnect(); } },
      { threshold: 0.2 }
    );
    if (ref.current) observer.observe(ref.current);
    return () => observer.disconnect();
  }, []);

  return (
    <div ref={ref} style={{ display: "flex", flexDirection: "column", gap: "14px" }}>
      {industriData.map((d, i) => (
        <div key={d.industri}>
          <div style={{ display: "flex", justifyContent: "space-between", marginBottom: "5px" }}>
            <span style={{ fontSize: "13px", color: "#ddd", fontWeight: "600" }}>{d.industri}</span>
            <span style={{ fontSize: "13px", color: d.warna, fontWeight: "700" }}>{d.dampak}%</span>
          </div>
          <div style={{ height: "8px", background: "#1a1a2e", borderRadius: "4px", overflow: "hidden" }}>
            <div style={{
              height: "100%", borderRadius: "4px",
              background: `linear-gradient(90deg, ${d.warna}99, ${d.warna})`,
              width: visible ? `${d.dampak}%` : "0%",
              transition: `width 0.9s cubic-bezier(0.34,1.56,0.64,1) ${0.1 * i}s`,
              boxShadow: `0 0 8px ${d.warna}66`,
            }} />
          </div>
        </div>
      ))}
    </div>
  );
}

function DonutChart() {
  const [visible, setVisible] = useState(false);
  const ref = useRef(null);
  const colors = ["#00B4FF", "#FF4444", "#00E5C3", "#7C5CFF", "#FFB347"];
  const size = 180;
  const cx = size / 2, cy = size / 2;
  const r = 68, innerR = 44;
  let cumAngle = -Math.PI / 2;

  const slices = investasiData.map((d, i) => {
    const frac = d.nilai / totalInvestasi;
    const startAngle = cumAngle;
    const endAngle = cumAngle + frac * 2 * Math.PI;
    cumAngle = endAngle;
    const x1 = cx + r * Math.cos(startAngle), y1 = cy + r * Math.sin(startAngle);
    const x2 = cx + r * Math.cos(endAngle), y2 = cy + r * Math.sin(endAngle);
    const xi1 = cx + innerR * Math.cos(endAngle), yi1 = cy + innerR * Math.sin(endAngle);
    const xi2 = cx + innerR * Math.cos(startAngle), yi2 = cy + innerR * Math.sin(startAngle);
    const lg = frac > 0.5 ? 1 : 0;
    const path = `M ${x1} ${y1} A ${r} ${r} 0 ${lg} 1 ${x2} ${y2} L ${xi1} ${yi1} A ${innerR} ${innerR} 0 ${lg} 0 ${xi2} ${yi2} Z`;
    return { path, color: colors[i], frac, label: d.wilayah };
  });

  useEffect(() => {
    const observer = new IntersectionObserver(
      ([e]) => { if (e.isIntersecting) { setVisible(true); observer.disconnect(); } },
      { threshold: 0.3 }
    );
    if (ref.current) observer.observe(ref.current);
    return () => observer.disconnect();
  }, []);

  return (
    <div ref={ref} style={{ display: "flex", alignItems: "center", gap: "24px", flexWrap: "wrap", justifyContent: "center" }}>
      <svg width={size} height={size} style={{ opacity: visible ? 1 : 0, transition: "opacity 0.6s", filter: "drop-shadow(0 0 18px #00B4FF44)" }}>
        {slices.map((s, i) => (
          <path key={i} d={s.path} fill={s.color} opacity="0.92"
            style={{ transition: `opacity 0.3s`, cursor: "pointer" }}
            onMouseEnter={e => e.target.setAttribute("opacity", "1")}
            onMouseLeave={e => e.target.setAttribute("opacity", "0.92")}
          />
        ))}
        <text x={cx} y={cy - 8} textAnchor="middle" fill="#fff" fontSize="18" fontWeight="700">$3.4T</text>
        <text x={cx} y={cy + 10} textAnchor="middle" fill="#aaa" fontSize="9">Total Global</text>
      </svg>
      <div style={{ display: "flex", flexDirection: "column", gap: "8px" }}>
        {investasiData.map((d, i) => (
          <div key={i} style={{ display: "flex", alignItems: "center", gap: "8px" }}>
            <div style={{ width: "10px", height: "10px", borderRadius: "2px", background: colors[i], flexShrink: 0 }} />
            <span style={{ fontSize: "12px", color: "#ccc" }}>{d.emoji} {d.wilayah}</span>
            <span style={{ fontSize: "12px", color: colors[i], fontWeight: "700", marginLeft: "auto" }}>${d.nilai}T</span>
          </div>
        ))}
      </div>
    </div>
  );
}

export default function App() {
  const [activeTab, setActiveTab] = useState(0);

  const cards = [
    { label: "Perusahaan Adopsi AI", value: 78, suffix: "%", decimals: 0, sub: "dari total perusahaan global", color: "#00E5C3" },
    { label: "Investasi Digital Global", value: 3.4, suffix: "T USD", decimals: 1, sub: "diproyeksikan 2026", color: "#00B4FF" },
    { label: "Transformasi via AI", value: 70, suffix: "%+", decimals: 0, sub: "dari seluruh proyek digital", color: "#7C5CFF" },
    { label: "Target Talenta Digital ID", value: 1, suffix: "Juta", decimals: 0, sub: "Indonesia akhir 2026", color: "#FFB347" },
  ];

  return (
    <div style={{
      minHeight: "100vh",
      background: "#080818",
      color: "#eee",
      fontFamily: "'Georgia', 'Times New Roman', serif",
      padding: "32px 16px",
    }}>
      <div style={{ maxWidth: "860px", margin: "0 auto" }}>

        {/* Header */}
        <div style={{
          textAlign: "center", marginBottom: "48px",
          borderBottom: "1px solid #ffffff18", paddingBottom: "32px",
        }}>
          <div style={{
            display: "inline-block", fontSize: "10px", letterSpacing: "4px",
            color: "#00E5C3", textTransform: "uppercase", fontFamily: "monospace",
            border: "1px solid #00E5C344", borderRadius: "2px", padding: "4px 12px", marginBottom: "16px"
          }}>Laporan Teknologi · Maret 2026</div>
          <h1 style={{
            fontSize: "clamp(28px, 5vw, 48px)", fontWeight: "700",
            color: "#fff", margin: "0 0 12px",
            lineHeight: 1.2,
          }}>{articles.title}</h1>
          <p style={{ fontSize: "clamp(14px, 2vw, 18px)", color: "#aaa", margin: 0 }}>{articles.subtitle}</p>
        </div>

        {/* Stat Cards */}
        <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(180px, 1fr))", gap: "16px", marginBottom: "48px" }}>
          {cards.map((c, i) => (
            <div key={i} style={{
              background: "#0d0d22", border: `1px solid ${c.color}33`,
              borderRadius: "10px", padding: "20px 16px",
              boxShadow: `0 0 20px ${c.color}11`,
            }}>
              <div style={{ fontSize: "clamp(24px, 4vw, 32px)", fontWeight: "700", color: c.color, fontFamily: "monospace" }}>
                <AnimatedNumber target={c.value} suffix={c.suffix} decimals={c.decimals} />
              </div>
              <div style={{ fontSize: "13px", fontWeight: "600", color: "#ddd", margin: "6px 0 4px" }}>{c.label}</div>
              <div style={{ fontSize: "11px", color: "#666" }}>{c.sub}</div>
            </div>
          ))}
        </div>

        {/* Charts Tabs */}
        <div style={{
          background: "#0d0d22", border: "1px solid #ffffff12",
          borderRadius: "12px", overflow: "hidden", marginBottom: "48px"
        }}>
          <div style={{ display: "flex", borderBottom: "1px solid #ffffff12" }}>
            {["📈 Adopsi AI Global", "🏭 Dampak per Industri", "💰 Investasi Digital"].map((tab, i) => (
              <button key={i} onClick={() => setActiveTab(i)} style={{
                flex: 1, padding: "14px 8px", border: "none", cursor: "pointer",
                fontSize: "12px", fontWeight: "600", fontFamily: "monospace",
                background: activeTab === i ? "#ffffff08" : "transparent",
                color: activeTab === i ? "#00E5C3" : "#666",
                borderBottom: activeTab === i ? "2px solid #00E5C3" : "2px solid transparent",
                transition: "all 0.2s",
              }}>{tab}</button>
            ))}
          </div>
          <div style={{ padding: "28px 24px" }}>
            {activeTab === 0 && (
              <div>
                <h3 style={{ margin: "0 0 6px", fontSize: "15px", color: "#fff" }}>Tingkat Adopsi AI oleh Perusahaan Global (%)</h3>
                <p style={{ margin: "0 0 20px", fontSize: "12px", color: "#666" }}>Sumber: McKinsey Global Survey 2021–2026* (proyeksi)</p>
                <BarChart />
              </div>
            )}
            {activeTab === 1 && (
              <div>
                <h3 style={{ margin: "0 0 6px", fontSize: "15px", color: "#fff" }}>Indeks Dampak AI per Sektor Industri</h3>
                <p style={{ margin: "0 0 20px", fontSize: "12px", color: "#666" }}>Skala transformasi & adopsi AI berdasarkan riset industri global 2026</p>
                <HorizontalBars />
              </div>
            )}
            {activeTab === 2 && (
              <div>
                <h3 style={{ margin: "0 0 6px", fontSize: "15px", color: "#fff" }}>Distribusi Investasi Transformasi Digital Global</h3>
                <p style={{ margin: "0 0 20px", fontSize: "12px", color: "#666" }}>Total proyeksi $3,4 Triliun USD — IDC 2026</p>
                <DonutChart />
              </div>
            )}
          </div>
        </div>

        {/* Tren Table */}
        <div style={{
          background: "#0d0d22", border: "1px solid #ffffff12",
          borderRadius: "12px", overflow: "hidden", marginBottom: "48px"
        }}>
          <div style={{ padding: "20px 24px", borderBottom: "1px solid #ffffff12" }}>
            <h3 style={{ margin: 0, fontSize: "16px", color: "#fff" }}>🔭 6 Tren AI Paling Berpengaruh 2026</h3>
            <p style={{ margin: "4px 0 0", fontSize: "12px", color: "#666" }}>Kompilasi riset Gartner, McKinsey & IDC</p>
          </div>
          <div style={{ overflowX: "auto" }}>
            <table style={{ width: "100%", borderCollapse: "collapse" }}>
              <thead>
                <tr style={{ background: "#ffffff06" }}>
                  {["#", "Tren", "Deskripsi Singkat", "Status", "Dampak"].map((h) => (
                    <th key={h} style={{
                      padding: "12px 16px", textAlign: "left",
                      fontSize: "11px", letterSpacing: "2px", textTransform: "uppercase",
                      color: "#666", fontFamily: "monospace", borderBottom: "1px solid #ffffff0e",
                      whiteSpace: "nowrap",
                    }}>{h}</th>
                  ))}
                </tr>
              </thead>
              <tbody>
                {trendData.map((row, i) => (
                  <tr key={i} style={{
                    borderBottom: "1px solid #ffffff08",
                    transition: "background 0.2s",
                  }}
                    onMouseEnter={e => e.currentTarget.style.background = "#ffffff05"}
                    onMouseLeave={e => e.currentTarget.style.background = "transparent"}
                  >
                    <td style={{ padding: "14px 16px", fontSize: "13px", color: "#444", fontFamily: "monospace", fontWeight: "700" }}>{row.no}</td>
                    <td style={{ padding: "14px 16px", fontSize: "14px", color: "#fff", fontWeight: "700", whiteSpace: "nowrap" }}>{row.tren}</td>
                    <td style={{ padding: "14px 16px", fontSize: "12px", color: "#999", maxWidth: "260px" }}>{row.deskripsi}</td>
                    <td style={{ padding: "14px 16px" }}>
                      <span style={{
                        fontSize: "11px", padding: "3px 10px", borderRadius: "20px",
                        background: `${row.statusColor}22`, color: row.statusColor,
                        border: `1px solid ${row.statusColor}44`, whiteSpace: "nowrap",
                        fontFamily: "monospace",
                      }}>{row.status}</span>
                    </td>
                    <td style={{ padding: "14px 16px", fontSize: "13px", color: "#bbb", whiteSpace: "nowrap" }}>{row.dampak}</td>
                  </tr>
                ))}
              </tbody>
            </table>
          </div>
        </div>

        {/* Footer */}
        <div style={{ textAlign: "center", fontSize: "11px", color: "#444", fontFamily: "monospace", letterSpacing: "1px" }}>
          DATA: MCKINSEY · GARTNER · IDC · KOMINFO · MARET 2026
        </div>
      </div>
    </div>
  );
}
