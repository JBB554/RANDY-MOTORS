import { useState, useEffect, useRef } from "react";

const CARS = [
  { id: 1, make: "Toyota", model: "Land Cruiser Prado", sub: "VX Full Option", year: 2021, price: 18500000, km: 42000, fuel: "Diesel", gearbox: "Automatic", cat: "SUV", color: "Pearl White", origin: "Japan", tag: "FEATURED", condition: 98, img: "🚙", desc: "Seven-seat commanding presence. V6 diesel, full leather, sunroof, all-terrain capability. Imported direct — priced 22% below Yaoundé market.", savings: 4200000, seats: 7, engine: "4.0L V6" },
  { id: 2, make: "BMW", model: "X5", sub: "xDrive30d M Sport", year: 2020, price: 22000000, km: 38000, fuel: "Diesel", gearbox: "Automatic", cat: "SUV", color: "Space Grey", origin: "Germany", tag: "PRESTIGE", condition: 96, img: "🏎️", desc: "The benchmark of luxury SUVs. Panoramic roof, Harman Kardon, massaging seats, 360 camera. A statement in every city.", savings: 5800000, seats: 5, engine: "3.0L Turbo" },
  { id: 3, make: "Mercedes-Benz", model: "C 200", sub: "AMG Line Edition", year: 2019, price: 14200000, km: 55000, fuel: "Petrol", gearbox: "Automatic", cat: "Saloon", color: "Obsidian Black", origin: "Germany", tag: "PRESTIGE", condition: 94, img: "🚗", desc: "German engineering distilled. Ambient lighting, panoramic sunroof, full Nappa leather. The most recognisable saloon on the road.", savings: 3100000, seats: 5, engine: "2.0L Turbo" },
  { id: 4, make: "Toyota", model: "Hilux", sub: "Double Cab SR5", year: 2020, price: 12800000, km: 61000, fuel: "Diesel", gearbox: "Manual", cat: "Pickup", color: "Silver Blade", origin: "Japan", tag: "BESTSELLER", condition: 91, img: "🛻", desc: "The undisputed king of Cameroonian roads. Built for extremes — trusted by NGOs, farms, and professionals across the north.", savings: 2400000, seats: 5, engine: "2.8L Diesel" },
  { id: 5, make: "Nissan", model: "X-Trail", sub: "Tekna+ 7 Seat", year: 2022, price: 11400000, km: 28000, fuel: "Petrol", gearbox: "Automatic", cat: "SUV", color: "Midnight Blue", origin: "Japan", tag: "NEW STOCK", condition: 99, img: "🚙", desc: "Low mileage, high specification. ProPilot Assist, 360° camera, intelligent all-wheel drive. Family-ready.", savings: 2900000, seats: 7, engine: "1.7L CVT" },
  { id: 6, make: "Toyota", model: "Fortuner", sub: "Legender V6 4x4", year: 2020, price: 15600000, km: 58000, fuel: "Diesel", gearbox: "Automatic", cat: "SUV", color: "Bronze Gold", origin: "Japan", tag: "", condition: 93, img: "🚙", desc: "Body-on-frame authority. Six cylinders of smooth power. From Yaoundé traffic to Bamenda mountain passes — flawless.", savings: 3300000, seats: 7, engine: "4.0L V6" },
  { id: 7, make: "Honda", model: "CR-V", sub: "EX-L AWD", year: 2022, price: 13200000, km: 19000, fuel: "Petrol", gearbox: "Automatic", cat: "SUV", color: "Sonic Grey", origin: "Japan", tag: "NEW STOCK", condition: 99, img: "🚙", desc: "Honda Sensing suite, panoramic roof, heated leather. Near-new condition. A refined family companion.", savings: 2800000, seats: 5, engine: "1.5L VTEC" },
  { id: 8, make: "Volkswagen", model: "Tiguan", sub: "R-Line 4Motion", year: 2022, price: 16800000, km: 24000, fuel: "Petrol", gearbox: "Automatic", cat: "SUV", color: "Deep Black", origin: "Germany", tag: "NEW STOCK", condition: 97, img: "🚗", desc: "Virtual cockpit, panoramic sunroof, R-Line sport bodykit. German build quality that appreciates over time.", savings: 3600000, seats: 5, engine: "2.0L TSI" },
  { id: 9, make: "Toyota", model: "Corolla", sub: "Touring Sports GR Sport", year: 2020, price: 6800000, km: 48000, fuel: "Petrol", gearbox: "Automatic", cat: "Saloon", color: "Classic White", origin: "Japan", tag: "BEST VALUE", condition: 90, img: "🚗", desc: "Proven reliability in a refined package. The perfect daily driver — economical, comfortable, enduring.", savings: 1200000, seats: 5, engine: "1.8L Hybrid" },
  { id: 10, make: "Mitsubishi", model: "L200", sub: "Barbarian X Edition", year: 2021, price: 10500000, km: 53000, fuel: "Diesel", gearbox: "Manual", cat: "Pickup", color: "Graphite Grey", origin: "Japan", tag: "", condition: 92, img: "🛻", desc: "Heavy-duty specification. Preferred by construction firms and development agencies operating across northern Cameroon.", savings: 2100000, seats: 5, engine: "2.4L Di-D" },
  { id: 11, make: "Toyota", model: "Hiace", sub: "GL Commuter 15-Seat", year: 2019, price: 8900000, km: 72000, fuel: "Diesel", gearbox: "Manual", cat: "Minivan", color: "Pearl White", origin: "Japan", tag: "", condition: 87, img: "🚐", desc: "The most trusted people carrier on the continent. Legendary Toyota reliability. Fleet operators trust nothing else.", savings: 1800000, seats: 15, engine: "2.7L Diesel" },
  { id: 12, make: "Peugeot", model: "3008", sub: "GT Line Premium", year: 2021, price: 9800000, km: 41000, fuel: "Diesel", gearbox: "Automatic", cat: "SUV", color: "Pearl White", origin: "France", tag: "", condition: 93, img: "🚙", desc: "i-Cockpit, Grip Control, full LED matrix. French elegance with genuine off-road credibility. Turns heads everywhere.", savings: 2200000, seats: 5, engine: "1.5L BlueHDi" },
];

const CATS = ["ALL", "SUV", "SALOON", "PICKUP", "MINIVAN"];

const fmt = (n) => new Intl.NumberFormat("fr-CM").format(n) + " XAF";
const fmtM = (n) => (n / 1000000).toFixed(1) + "M";
const fmtKm = (n) => new Intl.NumberFormat().format(n) + " km";

export default function RandyMotors() {
  const [view, setView] = useState("home");
  const [cat, setCat] = useState("ALL");
  const [budgetMax, setBudgetMax] = useState(25);
  const [search, setSearch] = useState("");
  const [detail, setDetail] = useState(null);
  const [saved, setSaved] = useState([]);
  const [compare, setCompare] = useState([]);
  const [inquiryCar, setInquiryCar] = useState(null);
  const [alertOpen, setAlertOpen] = useState(false);
  const [loginOpen, setLoginOpen] = useState(false);
  const [account, setAccount] = useState(null);
  const [toast, setToast] = useState(null);
  const [detailTab, setDetailTab] = useState("story");
  const [heroIdx, setHeroIdx] = useState(0);
  const [inquiryDone, setInquiryDone] = useState(false);

  const featuredCars = CARS.filter(c => c.tag);
  useEffect(() => {
    const t = setInterval(() => setHeroIdx(i => (i + 1) % featuredCars.length), 5000);
    return () => clearInterval(t);
  }, []);

  const notify = (msg) => { setToast(msg); setTimeout(() => setToast(null), 3000); };

  const toggleSaved = (id) => {
    setSaved(s => s.includes(id) ? s.filter(x => x !== id) : [...s, id]);
    notify(saved.includes(id) ? "Removed from collection" : "Added to your collection");
  };

  const toggleCompare = (car) => {
    if (compare.find(c => c.id === car.id)) { setCompare(c => c.filter(x => x.id !== car.id)); return; }
    if (compare.length >= 3) { notify("Maximum 3 vehicles for comparison"); return; }
    setCompare(c => [...c, car]);
    notify(`${car.make} ${car.model} added to comparison`);
  };

  const filtered = CARS.filter(c => {
    const bycat = cat === "ALL" || c.cat.toUpperCase() === cat;
    const bybud = c.price <= budgetMax * 1000000;
    const bysearch = !search || `${c.make} ${c.model} ${c.sub}`.toLowerCase().includes(search.toLowerCase());
    return bycat && bybud && bysearch;
  });

  const heroCar = featuredCars[heroIdx];

  // ── DETAIL VIEW ──
  if (detail) return (
    <div style={S.page}>
      <style>{CSS}</style>
      {toast && <div style={S.toast}>{toast}</div>}
      <header style={S.nav}>
        <button onClick={() => setDetail(null)} style={S.backBtn}>← INVENTORY</button>
        <div style={S.logo}>RANDY MOTORS</div>
        <div style={{ display:"flex", gap:12, alignItems:"center" }}>
          <button onClick={() => window.open(`https://wa.me/237600000000?text=Hi!%20Interested%20in%20the%20${detail.year}%20${detail.make}%20${detail.model}`,"_blank")} style={S.waBtn}>WhatsApp</button>
        </div>
      </header>
      <div style={{ display:"grid", gridTemplateColumns:"1fr 420px", maxWidth:1280, margin:"0 auto", padding:"48px 40px", gap:48 }}>
        <div>
          <div style={{ display:"flex", gap:12, marginBottom:20, flexWrap:"wrap" }}>
            {detail.tag && <span style={S.tagBadge}>{detail.tag}</span>}
            <span style={{ ...S.tagBadge, background:"rgba(255,255,255,0.05)", borderColor:"rgba(255,255,255,0.12)", color:"#a0a0a0" }}>🌍 {detail.origin}</span>
            <span style={{ ...S.tagBadge, background:"rgba(34,197,94,0.08)", borderColor:"rgba(34,197,94,0.25)", color:"#4ade80" }}>● {detail.condition}% CONDITION</span>
          </div>
          <div style={S.heroBox}>
            <div style={{ fontSize: 180, lineHeight:1, userSelect:"none" }}>{detail.img}</div>
            <div style={S.saveFab} onClick={() => toggleSaved(detail.id)}>{saved.includes(detail.id) ? "♥" : "♡"}</div>
          </div>
          <div style={{ display:"flex", gap:0, borderBottom:"1px solid rgba(255,255,255,0.07)", marginTop:32, marginBottom:28 }}>
            {["story","specs","finance","trust"].map(t => (
              <button key={t} onClick={() => setDetailTab(t)} style={{ padding:"12px 24px", background:"none", border:"none", borderBottom:`2px solid ${detailTab===t?"#c8a96e":"transparent"}`, color: detailTab===t ? "#c8a96e" : "#666", cursor:"pointer", fontSize:11, letterSpacing:3, fontFamily:"'DM Sans',sans-serif", fontWeight:700 }}>{t.toUpperCase()}</button>
            ))}
          </div>
          {detailTab === "story" && (
            <div>
              <p style={{ color:"#a0a0a0", lineHeight:1.85, fontSize:15, marginBottom:28 }}>{detail.desc}</p>
              <div style={S.infoBox}>
                <div style={{ fontSize:11, color:"#c8a96e", letterSpacing:3, marginBottom:14, fontWeight:700 }}>THE BORDER ADVANTAGE</div>
                <p style={{ color:"#888", fontSize:13, margin:0, lineHeight:1.7 }}>This vehicle was imported directly through our northern corridor — bypassing Douala port intermediaries. The result: you save <strong style={{ color:"#fff" }}>{fmt(detail.savings)}</strong> versus equivalent dealers in Yaoundé or Douala. Same vehicle. Real savings.</p>
              </div>
              <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:12, marginTop:16 }}>
                {["Full inspection certificate","Clean customs documentation","1-month powertrain warranty","Free delivery nationwide","Post-sale WhatsApp support","100K XAF referral bonus"].map(item => (
                  <div key={item} style={{ display:"flex", gap:10, alignItems:"center", fontSize:13, color:"#888" }}>
                    <span style={{ color:"#4ade80", fontSize:16 }}>✓</span>{item}
                  </div>
                ))}
              </div>
            </div>
          )}
          {detailTab === "specs" && (
            <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:10 }}>
              {[["Make",detail.make],["Model",detail.model],["Trim",detail.sub],["Year",detail.year],["Mileage",fmtKm(detail.km)],["Fuel",detail.fuel],["Gearbox",detail.gearbox],["Engine",detail.engine],["Seats",detail.seats],["Colour",detail.color],["Category",detail.cat],["Origin",detail.origin]].map(([k,v]) => (
                <div key={k} style={{ background:"rgba(255,255,255,0.03)", border:"1px solid rgba(255,255,255,0.07)", borderRadius:6, padding:"14px 16px" }}>
                  <div style={{ fontSize:10, color:"#555", letterSpacing:2, marginBottom:6 }}>{k.toUpperCase()}</div>
                  <div style={{ fontSize:14, fontWeight:600 }}>{v}</div>
                </div>
              ))}
            </div>
          )}
          {detailTab === "finance" && (
            <div>
              {[
                ["🏦","Bank Transfer","BICEC · Afriland · SCB · UBA. Full payment with Pro-Forma Invoice provided before transfer. Most common method for vehicles above 10M XAF."],
                ["💳","Card Payment","Visa / Mastercard. Secure processing. Confirmation within 2 hours of cleared funds."],
                ["📅","Instalment Plan","Partner financing via ACEP or CCA. Up to 36 months subject to eligibility. Ask our team."],
                ["📞","Negotiate Directly","Call or WhatsApp. We discuss, agree terms in writing, then proceed. Transparent at every step."],
              ].map(([icon, title, desc]) => (
                <div key={title} style={{ display:"flex", gap:18, padding:"20px 0", borderBottom:"1px solid rgba(255,255,255,0.06)" }}>
                  <span style={{ fontSize:28, flexShrink:0 }}>{icon}</span>
                  <div><div style={{ fontWeight:700, marginBottom:6, fontSize:14 }}>{title}</div><div style={{ fontSize:13, color:"#777", lineHeight:1.65 }}>{desc}</div></div>
                </div>
              ))}
              <div style={{ ...S.infoBox, borderColor:"rgba(34,197,94,0.2)", marginTop:20 }}>
                <div style={{ fontSize:11, color:"#4ade80", letterSpacing:2, marginBottom:8, fontWeight:700 }}>BUYER PROTECTION TIP</div>
                <p style={{ fontSize:13, color:"#888", margin:0, lineHeight:1.65 }}>Never transfer funds without a signed Pro-Forma Invoice from Randy Motors. We provide this as standard. A legitimate dealer always welcomes documentation — we insist on it.</p>
              </div>
            </div>
          )}
          {detailTab === "trust" && (
            <div>
              <div style={{ display:"grid", gridTemplateColumns:"repeat(2,1fr)", gap:12, marginBottom:24 }}>
                {[["200+","Vehicles Delivered"],["4.9 ★","Average Rating"],["48h","Delivery to Yaoundé"],["100%","Legal Documentation"]].map(([n,l]) => (
                  <div key={l} style={{ textAlign:"center", padding:"24px 16px", background:"rgba(200,169,110,0.05)", border:"1px solid rgba(200,169,110,0.15)", borderRadius:8 }}>
                    <div style={{ fontSize:26, fontWeight:800, color:"#c8a96e", marginBottom:4 }}>{n}</div>
                    <div style={{ fontSize:11, color:"#666", letterSpacing:1 }}>{l.toUpperCase()}</div>
                  </div>
                ))}
              </div>
              {[["Emmanuel K., Yaoundé","The Toyota Fortuner I bought is perfect. Import documentation was impeccable, price was 3.2M less than what I found in Douala. Randy Motors is the real deal."],["Fatimah A., NGO Director","We purchased a fleet of Hilux pickups. Professional, transparent, and delivered on schedule. Our go-to supplier now."]].map(([name, text]) => (
                <div key={name} style={{ padding:"20px 0", borderBottom:"1px solid rgba(255,255,255,0.06)" }}>
                  <p style={{ color:"#888", fontSize:14, lineHeight:1.75, fontStyle:"italic", margin:"0 0 10px" }}>"{text}"</p>
                  <div style={{ fontSize:12, color:"#c8a96e", fontWeight:700 }}>— {name}</div>
                </div>
              ))}
            </div>
          )}
        </div>
        <div style={{ position:"sticky", top:20, height:"fit-content" }}>
          <div style={S.priceSidebar}>
            <div style={{ fontSize:11, letterSpacing:3, color:"#666", marginBottom:12 }}>{detail.year} · {fmtKm(detail.km)} · {detail.fuel}</div>
            <div style={{ fontSize:13, color:"#a0a0a0", marginBottom:4 }}>{detail.make}</div>
            <div style={{ fontSize:30, fontWeight:800, lineHeight:1, marginBottom:4 }}>{detail.model}</div>
            <div style={{ fontSize:14, color:"#666", marginBottom:24 }}>{detail.sub}</div>
            <div style={{ fontSize:38, fontWeight:900, color:"#c8a96e", marginBottom:4, letterSpacing:-1 }}>{fmtM(detail.price)}</div>
            <div style={{ fontSize:12, color:"#555", marginBottom:6 }}>XAF {fmt(detail.price)}</div>
            <div style={{ fontSize:12, color:"#4ade80", marginBottom:28 }}>You save ≈ {fmtM(detail.savings)} vs. Yaoundé market</div>
            <button onClick={() => setInquiryCar(detail)} style={S.primaryBtn}>ENQUIRE ABOUT THIS VEHICLE</button>
            <button onClick={() => window.open(`https://wa.me/237600000000?text=Hello%20Randy%20Motors.%20I'm%20interested%20in%20the%20${detail.year}%20${detail.make}%20${detail.model}%20at%20${fmtM(detail.price)}%20XAF.`, "_blank")} style={S.waFullBtn}>📱 WHATSAPP NOW</button>
            <button onClick={() => window.open("tel:+237600000000")} style={S.ghostBtn}>📞 +237 6XX XXX XXX</button>
            <div style={{ marginTop:20, padding:16, background:"rgba(255,255,255,0.02)", border:"1px solid rgba(255,255,255,0.06)", borderRadius:8, fontSize:12, color:"#555", lineHeight:1.7 }}>
              💡 Our team responds within <strong style={{ color:"#a0a0a0" }}>2 hours</strong> via WhatsApp — every day. We'll send additional photos, video walkaround, and answer every question before you commit.
            </div>
          </div>
        </div>
      </div>
    </div>
  );

  // ── MAIN SITE ──
  return (
    <div style={S.page}>
      <style>{CSS}</style>
      {toast && <div style={S.toast}>{toast}</div>}

      {/* ENQUIRY MODAL */}
      {inquiryCar && (
        <div style={S.overlay}>
          <div style={S.modal}>
            {inquiryDone ? (
              <div style={{ textAlign:"center", padding:"20px 0" }}>
                <div style={{ fontSize:48, marginBottom:16 }}>✓</div>
                <div style={{ fontSize:20, fontWeight:800, marginBottom:8 }}>Enquiry Received</div>
                <p style={{ color:"#888", fontSize:13, lineHeight:1.7, marginBottom:24 }}>Our team will contact you via WhatsApp within 2 hours. We'll send photos, video, and answer every question.</p>
                <button onClick={() => { setInquiryCar(null); setInquiryDone(false); }} style={S.primaryBtn}>Close</button>
              </div>
            ) : (
              <div>
                <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start", marginBottom:24 }}>
                  <div>
                    <div style={{ fontSize:18, fontWeight:800, marginBottom:4 }}>{inquiryCar.make} {inquiryCar.model}</div>
                    <div style={{ fontSize:13, color:"#c8a96e" }}>{fmtM(inquiryCar.price)} XAF · {inquiryCar.year}</div>
                  </div>
                  <button onClick={() => setInquiryCar(null)} style={{ background:"none", border:"none", color:"#555", cursor:"pointer", fontSize:22, lineHeight:1 }}>×</button>
                </div>
                {[["Full Name",""],["WhatsApp / Phone","+237 6XX XXX XXX"],["Your City","Yaoundé, Douala..."],["Message","Questions, inspection request, delivery details..."]].map(([l,p]) => (
                  <div key={l} style={{ marginBottom:14 }}>
                    <label style={{ display:"block", fontSize:10, color:"#555", letterSpacing:2, marginBottom:6 }}>{l.toUpperCase()}</label>
                    {l === "Message"
                      ? <textarea placeholder={p} rows={3} style={S.input} />
                      : <input placeholder={p} style={S.input} />}
                  </div>
                ))}
                <div style={{ background:"rgba(34,197,94,0.04)", border:"1px solid rgba(34,197,94,0.15)", borderRadius:6, padding:14, marginBottom:20 }}>
                  <div style={{ fontSize:10, color:"#4ade80", letterSpacing:2, marginBottom:6 }}>WHAT HAPPENS NEXT</div>
                  <div style={{ fontSize:12, color:"#777", lineHeight:1.65 }}>We confirm availability → send video walkaround → agree terms in writing → arrange delivery or collection. No pressure.</div>
                </div>
                <button onClick={() => setInquiryDone(true)} style={S.primaryBtn}>SEND ENQUIRY</button>
              </div>
            )}
          </div>
        </div>
      )}

      {/* PRICE ALERT MODAL */}
      {alertOpen && (
        <div style={S.overlay}>
          <div style={S.modal}>
            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:24 }}>
              <div style={{ fontSize:18, fontWeight:800 }}>Vehicle Alert</div>
              <button onClick={() => setAlertOpen(false)} style={{ background:"none", border:"none", color:"#555", cursor:"pointer", fontSize:22 }}>×</button>
            </div>
            <p style={{ color:"#888", fontSize:13, lineHeight:1.7, marginBottom:20 }}>Tell us exactly what you're looking for. We'll notify you the moment it arrives in our inventory.</p>
            {[["Vehicle type / model","e.g. Toyota Hilux, BMW X5, SUV under 12M"],["Budget ceiling","e.g. 15,000,000 XAF"],["WhatsApp number","+237 6XX XXX XXX"]].map(([l,p]) => (
              <div key={l} style={{ marginBottom:14 }}>
                <label style={{ display:"block", fontSize:10, color:"#555", letterSpacing:2, marginBottom:6 }}>{l.toUpperCase()}</label>
                <input placeholder={p} style={S.input} />
              </div>
            ))}
            <button onClick={() => { setAlertOpen(false); notify("Alert set — we'll WhatsApp you when it arrives ✓"); }} style={S.primaryBtn}>SET ALERT</button>
          </div>
        </div>
      )}

      {/* ACCOUNT MODAL */}
      {loginOpen && (
        <div style={S.overlay}>
          <div style={S.modal}>
            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:24 }}>
              <div style={{ fontSize:18, fontWeight:800 }}>Join Randy Motors</div>
              <button onClick={() => setLoginOpen(false)} style={{ background:"none", border:"none", color:"#555", cursor:"pointer", fontSize:22 }}>×</button>
            </div>
            <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:12, marginBottom:20 }}>
              {[["🔔","Price alerts"],["♡","Save vehicles"],["⚡","Early access"],["🎁","100K referral"]].map(([i,l]) => (
                <div key={l} style={{ padding:"12px 14px", background:"rgba(200,169,110,0.05)", border:"1px solid rgba(200,169,110,0.12)", borderRadius:6, fontSize:13, color:"#a0a0a0" }}><span style={{ marginRight:8 }}>{i}</span>{l}</div>
              ))}
            </div>
            {["Full Name","Email Address","WhatsApp Number","Password"].map(l => (
              <div key={l} style={{ marginBottom:12 }}>
                <input placeholder={l} type={l === "Password" ? "password" : "text"} style={S.input} />
              </div>
            ))}
            <button onClick={() => { setAccount("Member"); setLoginOpen(false); notify("Welcome to Randy Motors ✓"); }} style={S.primaryBtn}>CREATE ACCOUNT</button>
          </div>
        </div>
      )}

      {/* NAV */}
      <header style={S.nav}>
        <div style={{ display:"flex", alignItems:"center", gap:12 }}>
          <div style={{ width:38, height:38, background:"#c8a96e", borderRadius:6, display:"flex", alignItems:"center", justifyContent:"center", fontSize:20 }}>🏎️</div>
          <div>
            <div style={{ fontWeight:900, fontSize:15, letterSpacing:2, lineHeight:1 }}>RANDY MOTORS</div>
            <div style={{ fontSize:9, color:"#555", letterSpacing:3, marginTop:1 }}>MAROUA · CAMEROUN</div>
          </div>
        </div>
        <div style={{ display:"flex", gap:6, background:"rgba(255,255,255,0.03)", border:"1px solid rgba(255,255,255,0.07)", borderRadius:8, padding:4 }}>
          {["HOME","INVENTORY","SAVED"].map(p => (
            <button key={p} onClick={() => setView(p.toLowerCase())} style={{ padding:"7px 18px", background: view === p.toLowerCase() ? "rgba(200,169,110,0.12)" : "none", border: view === p.toLowerCase() ? "1px solid rgba(200,169,110,0.25)" : "1px solid transparent", color: view === p.toLowerCase() ? "#c8a96e" : "#666", cursor:"pointer", fontSize:11, letterSpacing:2, borderRadius:6, fontFamily:"'DM Sans',sans-serif", fontWeight:700 }}>
              {p}{p === "SAVED" && saved.length > 0 && ` (${saved.length})`}
            </button>
          ))}
        </div>
        <div style={{ display:"flex", gap:10, alignItems:"center" }}>
          {compare.length > 0 && <button onClick={() => setView("compare")} style={{ padding:"8px 14px", background:"rgba(200,169,110,0.08)", border:"1px solid rgba(200,169,110,0.25)", color:"#c8a96e", cursor:"pointer", borderRadius:6, fontSize:11, letterSpacing:1, fontWeight:700 }}>COMPARE ({compare.length})</button>}
          <button onClick={() => setAlertOpen(true)} style={{ padding:"8px 14px", background:"transparent", border:"1px solid rgba(255,255,255,0.1)", color:"#888", cursor:"pointer", borderRadius:6, fontSize:11, letterSpacing:1 }}>🔔 ALERT</button>
          <button onClick={() => setLoginOpen(true)} style={{ padding:"8px 18px", background:"#c8a96e", border:"none", color:"#0a0a0a", cursor:"pointer", borderRadius:6, fontSize:11, letterSpacing:1, fontWeight:900 }}>{account ? `✓ ${account}` : "JOIN"}</button>
        </div>
      </header>

      {/* HOME */}
      {view === "home" && (
        <div>
          {/* CINEMATIC HERO */}
          <div style={S.hero} key={heroIdx} className="hero-fade">
            <div style={{ position:"absolute", inset:0, background:`radial-gradient(ellipse at 60% 50%, rgba(200,169,110,0.06) 0%, transparent 65%)` }}></div>
            <div style={{ position:"absolute", bottom:0, left:0, right:0, height:200, background:"linear-gradient(to top, #0a0a0a, transparent)" }}></div>
            <div style={{ position:"relative", zIndex:2, maxWidth:1280, margin:"0 auto", padding:"0 40px", display:"grid", gridTemplateColumns:"1fr 1fr", alignItems:"center", height:"100%" }}>
              <div>
                <div style={{ fontSize:10, letterSpacing:5, color:"#c8a96e", marginBottom:20, fontWeight:700 }} className="hero-text">NORTH CAMEROON'S FINEST</div>
                <div style={{ fontSize:68, fontWeight:900, lineHeight:0.95, marginBottom:8, letterSpacing:-2 }} className="hero-text">{heroCar.make}</div>
                <div style={{ fontSize:52, fontWeight:300, color:"#888", lineHeight:1, marginBottom:4, letterSpacing:-1 }} className="hero-text">{heroCar.model}</div>
                <div style={{ fontSize:16, color:"#666", marginBottom:32, letterSpacing:1 }} className="hero-text">{heroCar.sub}</div>
                <div style={{ display:"flex", gap:16, alignItems:"center", marginBottom:40 }} className="hero-text">
                  <div style={{ fontSize:40, fontWeight:900, color:"#c8a96e", letterSpacing:-1 }}>{fmtM(heroCar.price)}</div>
                  <div>
                    <div style={{ fontSize:11, color:"#444", letterSpacing:1 }}>XAF · BORDER PRICE</div>
                    <div style={{ fontSize:11, color:"#4ade80" }}>Save ≈ {fmtM(heroCar.savings)} vs. market</div>
                  </div>
                </div>
                <div style={{ display:"flex", gap:12 }} className="hero-text">
                  <button onClick={() => setDetail(heroCar)} style={S.primaryBtn}>VIEW THIS VEHICLE</button>
                  <button onClick={() => setInquiryCar(heroCar)} style={S.ghostBtnLight}>ENQUIRE NOW</button>
                </div>
              </div>
              <div style={{ display:"flex", alignItems:"center", justifyContent:"center", fontSize:200, opacity:0.85, filter:"drop-shadow(0 40px 80px rgba(200,169,110,0.15))" }} className="hero-img">{heroCar.img}</div>
            </div>
            <div style={{ position:"absolute", bottom:32, left:"50%", transform:"translateX(-50%)", display:"flex", gap:8, zIndex:3 }}>
              {featuredCars.map((_, i) => <div key={i} onClick={() => setHeroIdx(i)} style={{ width: i === heroIdx ? 24 : 8, height:8, borderRadius:4, background: i === heroIdx ? "#c8a96e" : "#333", cursor:"pointer", transition:"all 0.3s" }} />)}
            </div>
          </div>

          {/* METRICS STRIP */}
          <div style={{ display:"grid", gridTemplateColumns:"repeat(4,1fr)", borderTop:"1px solid rgba(255,255,255,0.05)", borderBottom:"1px solid rgba(255,255,255,0.05)" }}>
            {[["200+","Vehicles sold","in 3 years"],["15–35%","Below market price","guaranteed"],["48h","Delivery","to Yaoundé & Douala"],["100%","Legal documentation","on every vehicle"]].map(([n,l,s]) => (
              <div key={l} style={{ padding:"32px 24px", borderRight:"1px solid rgba(255,255,255,0.04)", textAlign:"center" }}>
                <div style={{ fontSize:30, fontWeight:900, color:"#c8a96e", letterSpacing:-1, marginBottom:4 }}>{n}</div>
                <div style={{ fontSize:12, fontWeight:700, marginBottom:2, letterSpacing:1 }}>{l.toUpperCase()}</div>
                <div style={{ fontSize:11, color:"#555" }}>{s}</div>
              </div>
            ))}
          </div>

          {/* FEATURED GRID — asymmetric layout */}
          <div style={{ maxWidth:1280, margin:"0 auto", padding:"72px 40px" }}>
            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-end", marginBottom:48 }}>
              <div>
                <div style={{ fontSize:11, color:"#c8a96e", letterSpacing:4, marginBottom:12, fontWeight:700 }}>CURRENT INVENTORY</div>
                <h2 style={{ fontSize:40, fontWeight:900, margin:0, letterSpacing:-1, lineHeight:1 }}>This Week's<br /><span style={{ color:"#c8a96e" }}>Premier Selection</span></h2>
              </div>
              <button onClick={() => setView("inventory")} style={{ padding:"12px 24px", background:"transparent", border:"1px solid rgba(255,255,255,0.12)", color:"#888", cursor:"pointer", borderRadius:6, fontSize:11, letterSpacing:2, fontWeight:700 }}>FULL INVENTORY →</button>
            </div>
            <div style={{ display:"grid", gridTemplateColumns:"2fr 1fr", gridTemplateRows:"auto auto", gap:16 }}>
              {/* Large feature */}
              <div style={{ ...S.carCard, gridRow:"1 / 3", cursor:"pointer" }} onClick={() => setDetail(CARS[0])}>
                <div style={{ height:340, display:"flex", alignItems:"center", justifyContent:"center", fontSize:180, background:"rgba(255,255,255,0.02)", position:"relative" }}>
                  {CARS[0].img}
                  <div style={S.cardTag}>{CARS[0].tag}</div>
                  <button onClick={e => { e.stopPropagation(); toggleSaved(CARS[0].id); }} style={S.heartBtn}>{saved.includes(CARS[0].id) ? "♥" : "♡"}</button>
                </div>
                <div style={{ padding:"24px 28px" }}>
                  <div style={{ fontSize:11, color:"#555", letterSpacing:2, marginBottom:8 }}>{CARS[0].year} · {fmtKm(CARS[0].km)} · {CARS[0].fuel}</div>
                  <div style={{ fontSize:24, fontWeight:900, marginBottom:4 }}>{CARS[0].make} {CARS[0].model}</div>
                  <div style={{ fontSize:13, color:"#666", marginBottom:20 }}>{CARS[0].sub}</div>
                  <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-end" }}>
                    <div>
                      <div style={{ fontSize:28, fontWeight:900, color:"#c8a96e", letterSpacing:-1 }}>{fmtM(CARS[0].price)}</div>
                      <div style={{ fontSize:11, color:"#4ade80" }}>Save ≈ {fmtM(CARS[0].savings)}</div>
                    </div>
                    <div style={{ display:"flex", gap:8 }}>
                      <button onClick={e => { e.stopPropagation(); toggleCompare(CARS[0]); }} style={{ padding:"8px 12px", background:"rgba(255,255,255,0.04)", border:"1px solid rgba(255,255,255,0.1)", color:"#888", cursor:"pointer", borderRadius:4, fontSize:12 }}>⚖</button>
                      <button onClick={e => { e.stopPropagation(); setInquiryCar(CARS[0]); }} style={{ padding:"8px 20px", background:"#c8a96e", border:"none", color:"#0a0a0a", cursor:"pointer", borderRadius:4, fontSize:12, fontWeight:900 }}>ENQUIRE</button>
                    </div>
                  </div>
                </div>
              </div>
              {/* 2 smaller */}
              {[CARS[1], CARS[3]].map(car => (
                <div key={car.id} style={{ ...S.carCard, cursor:"pointer" }} onClick={() => setDetail(car)}>
                  <div style={{ height:160, display:"flex", alignItems:"center", justifyContent:"center", fontSize:90, background:"rgba(255,255,255,0.02)", position:"relative" }}>
                    {car.img}
                    {car.tag && <div style={S.cardTag}>{car.tag}</div>}
                    <button onClick={e => { e.stopPropagation(); toggleSaved(car.id); }} style={S.heartBtn}>{saved.includes(car.id) ? "♥" : "♡"}</button>
                  </div>
                  <div style={{ padding:"18px 20px" }}>
                    <div style={{ fontSize:10, color:"#555", letterSpacing:1, marginBottom:6 }}>{car.year} · {fmtKm(car.km)}</div>
                    <div style={{ fontSize:16, fontWeight:900, marginBottom:2 }}>{car.make} {car.model}</div>
                    <div style={{ fontSize:11, color:"#555", marginBottom:12 }}>{car.sub}</div>
                    <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center" }}>
                      <div style={{ fontSize:20, fontWeight:900, color:"#c8a96e" }}>{fmtM(car.price)}</div>
                      <button onClick={e => { e.stopPropagation(); setInquiryCar(car); }} style={{ padding:"7px 14px", background:"#c8a96e", border:"none", color:"#0a0a0a", cursor:"pointer", borderRadius:4, fontSize:11, fontWeight:900 }}>ENQUIRE</button>
                    </div>
                  </div>
                </div>
              ))}
            </div>
          </div>

          {/* WHY RANDY */}
          <div style={{ background:"rgba(255,255,255,0.015)", borderTop:"1px solid rgba(255,255,255,0.05)", borderBottom:"1px solid rgba(255,255,255,0.05)", padding:"72px 40px" }}>
            <div style={{ maxWidth:1280, margin:"0 auto" }}>
              <div style={{ textAlign:"center", marginBottom:56 }}>
                <div style={{ fontSize:11, color:"#c8a96e", letterSpacing:4, marginBottom:12, fontWeight:700 }}>THE RANDY MOTORS DIFFERENCE</div>
                <h2 style={{ fontSize:40, fontWeight:900, margin:0, letterSpacing:-1 }}>Why the World's Smartest<br />Buyers Choose Us</h2>
              </div>
              <div style={{ display:"grid", gridTemplateColumns:"repeat(3,1fr)", gap:2 }}>
                {[["🌍","Border Access","Our Maroua location grants direct access to import corridors that bypass Douala port. That saving — 15 to 35% — passes directly to you."],["📋","Full Documentation","Every vehicle: Pro-Forma Invoice, customs clearance, inspection certificate. 100% transparent, 100% legal. No exceptions."],["🚚","Nationwide Delivery","We deliver to Yaoundé, Douala, Bafoussam, Ngaoundéré and every major city. Tracked. Confirmed on WhatsApp before departure."],["🔧","Pre-Sale Inspection","Every listing reflects an honest mechanical assessment. Our condition rating is our reputation — we don't inflate it."],["💬","WhatsApp-First","Video walkaround, price discussion, documentation — all via WhatsApp. We respond within 2 hours, 7 days a week."],["🎁","Referral Programme","Introduce a buyer who completes a purchase: earn 100,000 XAF cash. Refer a fleet: earn more."]].map(([icon, title, desc]) => (
                  <div key={title} style={{ padding:"36px 32px", borderRight:"1px solid rgba(255,255,255,0.04)" }}>
                    <div style={{ fontSize:36, marginBottom:20 }}>{icon}</div>
                    <div style={{ fontSize:15, fontWeight:800, marginBottom:12, letterSpacing:0.5 }}>{title}</div>
                    <div style={{ fontSize:13, color:"#666", lineHeight:1.75 }}>{desc}</div>
                  </div>
                ))}
              </div>
            </div>
          </div>

          {/* ALERT STRIP */}
          <div style={{ padding:"48px 40px", display:"flex", alignItems:"center", justifyContent:"space-between", maxWidth:1280, margin:"0 auto", gap:40 }}>
            <div>
              <div style={{ fontSize:11, color:"#c8a96e", letterSpacing:3, marginBottom:8, fontWeight:700 }}>CAN'T FIND YOUR VEHICLE?</div>
              <div style={{ fontSize:28, fontWeight:900, letterSpacing:-0.5 }}>Set an alert. We'll find it.</div>
              <div style={{ fontSize:14, color:"#666", marginTop:8 }}>Tell us make, model, and budget. We notify you the moment it enters our inventory.</div>
            </div>
            <button onClick={() => setAlertOpen(true)} style={{ flexShrink:0, padding:"16px 40px", background:"#c8a96e", border:"none", color:"#0a0a0a", fontWeight:900, fontSize:13, letterSpacing:2, cursor:"pointer", borderRadius:6, whiteSpace:"nowrap" }}>SET VEHICLE ALERT</button>
          </div>
        </div>
      )}

      {/* INVENTORY PAGE */}
      {(view === "inventory" || view === "saved") && (
        <div style={{ maxWidth:1280, margin:"0 auto", padding:"48px 40px" }}>
          <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-end", marginBottom:40 }}>
            <div>
              <div style={{ fontSize:11, color:"#c8a96e", letterSpacing:4, marginBottom:8, fontWeight:700 }}>{view === "saved" ? "YOUR COLLECTION" : "INVENTORY"}</div>
              <h2 style={{ fontSize:36, fontWeight:900, margin:0, letterSpacing:-1 }}>{view === "saved" ? "Saved Vehicles" : "All Vehicles"} <span style={{ fontSize:16, color:"#555", fontWeight:400 }}>({view === "saved" ? saved.length : filtered.length})</span></h2>
            </div>
            <div style={{ display:"flex", gap:10, alignItems:"center", flexWrap:"wrap" }}>
              <input value={search} onChange={e => setSearch(e.target.value)} placeholder="Search make, model..." style={{ ...S.input, width:200, margin:0 }} />
              <div style={{ display:"flex", gap:4 }}>
                {CATS.map(c => <button key={c} onClick={() => setCat(c)} style={{ padding:"8px 14px", background: cat === c ? "rgba(200,169,110,0.1)" : "transparent", border: `1px solid ${cat === c ? "rgba(200,169,110,0.4)" : "rgba(255,255,255,0.08)"}`, color: cat === c ? "#c8a96e" : "#666", cursor:"pointer", borderRadius:4, fontSize:10, letterSpacing:1, fontWeight:700 }}>{c}</button>)}
              </div>
              <div style={{ display:"flex", gap:8, alignItems:"center" }}>
                <span style={{ fontSize:11, color:"#555", whiteSpace:"nowrap" }}>Max: {budgetMax}M XAF</span>
                <input type="range" min={5} max={25} value={budgetMax} onChange={e => setBudgetMax(+e.target.value)} style={{ width:80 }} />
              </div>
            </div>
          </div>
          <div style={{ display:"grid", gridTemplateColumns:"repeat(3,1fr)", gap:16 }}>
            {(view === "saved" ? CARS.filter(c => saved.includes(c.id)) : filtered).map(car => (
              <div key={car.id} style={{ ...S.carCard, cursor:"pointer" }} onClick={() => setDetail(car)} className="card-hover">
                <div style={{ height:200, display:"flex", alignItems:"center", justifyContent:"center", fontSize:100, background:"rgba(255,255,255,0.02)", position:"relative" }}>
                  {car.img}
                  {car.tag && <div style={S.cardTag}>{car.tag}</div>}
                  <button onClick={e => { e.stopPropagation(); toggleSaved(car.id); }} style={S.heartBtn}>{saved.includes(car.id) ? "♥" : "♡"}</button>
                  <div style={{ position:"absolute", bottom:10, left:12, fontSize:10, color: car.condition >= 96 ? "#4ade80" : car.condition >= 92 ? "#a3e635" : "#fbbf24", fontWeight:700, letterSpacing:1 }}>● {car.condition}% CONDITION</div>
                </div>
                <div style={{ padding:"20px 22px" }}>
                  <div style={{ fontSize:10, color:"#555", letterSpacing:1, marginBottom:8 }}>{car.year} · {fmtKm(car.km)} · {car.fuel} · {car.gearbox}</div>
                  <div style={{ fontSize:17, fontWeight:900, marginBottom:2 }}>{car.make} {car.model}</div>
                  <div style={{ fontSize:12, color:"#555", marginBottom:16 }}>{car.sub}</div>
                  <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-end" }}>
                    <div>
                      <div style={{ fontSize:22, fontWeight:900, color:"#c8a96e", letterSpacing:-0.5 }}>{fmtM(car.price)}</div>
                      <div style={{ fontSize:10, color:"#4ade80" }}>Save ≈ {fmtM(car.savings)}</div>
                    </div>
                    <div style={{ display:"flex", gap:6 }}>
                      <button onClick={e => { e.stopPropagation(); toggleCompare(car); }} style={{ padding:"7px 10px", background: compare.find(c => c.id === car.id) ? "rgba(200,169,110,0.1)" : "rgba(255,255,255,0.04)", border:`1px solid ${compare.find(c => c.id === car.id) ? "rgba(200,169,110,0.4)" : "rgba(255,255,255,0.1)"}`, color:"#888", cursor:"pointer", borderRadius:4, fontSize:12 }}>⚖</button>
                      <button onClick={e => { e.stopPropagation(); setInquiryCar(car); }} style={{ padding:"7px 16px", background:"#c8a96e", border:"none", color:"#0a0a0a", cursor:"pointer", borderRadius:4, fontSize:11, fontWeight:900 }}>ENQUIRE</button>
                    </div>
                  </div>
                </div>
              </div>
            ))}
          </div>
          {view === "saved" && saved.length === 0 && <div style={{ textAlign:"center", color:"#444", padding:"80px 0", fontSize:15 }}>No saved vehicles yet.<br /><span style={{ fontSize:13, color:"#333" }}>Tap ♡ on any listing to build your collection.</span></div>}
        </div>
      )}

      {/* COMPARE PAGE */}
      {view === "compare" && compare.length > 0 && (
        <div style={{ maxWidth:1280, margin:"0 auto", padding:"48px 40px" }}>
          <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:40 }}>
            <div>
              <div style={{ fontSize:11, color:"#c8a96e", letterSpacing:4, marginBottom:8, fontWeight:700 }}>SIDE BY SIDE</div>
              <h2 style={{ fontSize:36, fontWeight:900, margin:0, letterSpacing:-1 }}>Compare Vehicles</h2>
            </div>
            <button onClick={() => setCompare([])} style={{ padding:"9px 18px", background:"transparent", border:"1px solid rgba(255,255,255,0.1)", color:"#888", cursor:"pointer", borderRadius:6, fontSize:11, letterSpacing:1 }}>CLEAR ALL</button>
          </div>
          <div style={{ display:"grid", gridTemplateColumns:`repeat(${compare.length},1fr)`, gap:16 }}>
            {compare.map(car => (
              <div key={car.id} style={S.carCard}>
                <div style={{ height:200, display:"flex", alignItems:"center", justifyContent:"center", fontSize:100, background:"rgba(255,255,255,0.02)" }}>{car.img}</div>
                <div style={{ padding:"20px 22px" }}>
                  <div style={{ fontSize:17, fontWeight:900, marginBottom:2 }}>{car.make} {car.model}</div>
                  <div style={{ fontSize:12, color:"#555", marginBottom:16 }}>{car.sub}</div>
                  <div style={{ fontSize:24, fontWeight:900, color:"#c8a96e", marginBottom:20 }}>{fmtM(car.price)}</div>
                  {[["Year",car.year],["Mileage",fmtKm(car.km)],["Fuel",car.fuel],["Gearbox",car.gearbox],["Engine",car.engine],["Condition",`${car.condition}%`],["Origin",car.origin]].map(([k,v]) => (
                    <div key={k} style={{ display:"flex", justifyContent:"space-between", padding:"9px 0", borderBottom:"1px solid rgba(255,255,255,0.05)", fontSize:13 }}>
                      <span style={{ color:"#555" }}>{k}</span>
                      <span style={{ fontWeight:600 }}>{v}</span>
                    </div>
                  ))}
                  <button onClick={() => setInquiryCar(car)} style={{ ...S.primaryBtn, marginTop:20 }}>ENQUIRE</button>
                </div>
              </div>
            ))}
          </div>
        </div>
      )}

      {/* WHATSAPP FAB */}
      <button onClick={() => window.open("https://wa.me/237600000000?text=Hello%20Randy%20Motors!%20I%20am%20interested%20in%20one%20of%20your%20vehicles.", "_blank")} style={{ position:"fixed", bottom:28, right:32, width:58, height:58, borderRadius:"50%", background:"#25D366", border:"none", fontSize:26, cursor:"pointer", zIndex:400, boxShadow:"0 8px 32px rgba(37,211,102,0.35)", display:"flex", alignItems:"center", justifyContent:"center" }}>📱</button>
    </div>
  );
}

const S = {
  page: { minHeight:"100vh", background:"#0a0a0a", color:"#fff", fontFamily:"'DM Sans', 'Helvetica Neue', sans-serif" },
  nav: { background:"rgba(10,10,10,0.95)", backdropFilter:"blur(20px)", borderBottom:"1px solid rgba(255,255,255,0.06)", padding:"0 40px", height:64, display:"flex", alignItems:"center", justifyContent:"space-between", position:"sticky", top:0, zIndex:100 },
  logo: { fontWeight:900, fontSize:13, letterSpacing:4, color:"#c8a96e" },
  hero: { height:680, position:"relative", overflow:"hidden", background:"#0a0a0a", borderBottom:"1px solid rgba(255,255,255,0.05)" },
  heroBox: { background:"rgba(255,255,255,0.025)", border:"1px solid rgba(255,255,255,0.07)", borderRadius:12, display:"flex", alignItems:"center", justifyContent:"center", height:440, fontSize:200, position:"relative", overflow:"hidden" },
  carCard: { background:"rgba(255,255,255,0.025)", border:"1px solid rgba(255,255,255,0.07)", borderRadius:10, overflow:"hidden", transition:"border-color 0.2s" },
  cardTag: { position:"absolute", top:12, left:12, background:"rgba(200,169,110,0.12)", border:"1px solid rgba(200,169,110,0.35)", color:"#c8a96e", fontSize:9, fontWeight:900, padding:"3px 10px", borderRadius:4, letterSpacing:2 },
  heartBtn: { position:"absolute", top:10, right:10, background:"rgba(0,0,0,0.6)", border:"none", color:"#c8a96e", cursor:"pointer", fontSize:18, borderRadius:"50%", width:32, height:32, display:"flex", alignItems:"center", justifyContent:"center" },
  saveFab: { position:"absolute", top:16, right:16, background:"rgba(0,0,0,0.7)", border:"1px solid rgba(200,169,110,0.3)", color:"#c8a96e", cursor:"pointer", fontSize:22, borderRadius:"50%", width:44, height:44, display:"flex", alignItems:"center", justifyContent:"center" },
  tagBadge: { display:"inline-block", padding:"4px 12px", background:"rgba(200,169,110,0.08)", border:"1px solid rgba(200,169,110,0.25)", color:"#c8a96e", fontSize:10, fontWeight:900, borderRadius:4, letterSpacing:2 },
  infoBox: { background:"rgba(200,169,110,0.04)", border:"1px solid rgba(200,169,110,0.15)", borderRadius:8, padding:"18px 20px" },
  priceSidebar: { background:"rgba(255,255,255,0.025)", border:"1px solid rgba(255,255,255,0.07)", borderRadius:12, padding:32 },
  primaryBtn: { width:"100%", padding:"15px", background:"#c8a96e", border:"none", color:"#0a0a0a", fontWeight:900, letterSpacing:2, cursor:"pointer", borderRadius:6, marginBottom:10, fontSize:12, display:"block", boxSizing:"border-box", fontFamily:"'DM Sans', sans-serif" },
  waFullBtn: { width:"100%", padding:"13px", background:"transparent", border:"1px solid #25D366", color:"#25D366", fontWeight:800, cursor:"pointer", borderRadius:6, marginBottom:10, fontSize:12, display:"block", boxSizing:"border-box", letterSpacing:1 },
  waBtn: { padding:"8px 16px", background:"transparent", border:"1px solid #25D366", color:"#25D366", cursor:"pointer", borderRadius:6, fontSize:11, fontWeight:700, letterSpacing:1 },
  ghostBtn: { width:"100%", padding:"12px", background:"transparent", border:"1px solid rgba(255,255,255,0.1)", color:"#888", cursor:"pointer", borderRadius:6, fontSize:12, display:"block", boxSizing:"border-box" },
  ghostBtnLight: { padding:"15px 28px", background:"transparent", border:"1px solid rgba(255,255,255,0.15)", color:"#a0a0a0", cursor:"pointer", borderRadius:6, fontSize:12, fontWeight:700, letterSpacing:2, fontFamily:"'DM Sans', sans-serif" },
  backBtn: { background:"none", border:"1px solid rgba(255,255,255,0.1)", color:"#888", padding:"7px 16px", cursor:"pointer", borderRadius:6, fontFamily:"'DM Sans', sans-serif", fontSize:11, letterSpacing:1 },
  overlay: { position:"fixed", inset:0, background:"rgba(0,0,0,0.88)", display:"flex", alignItems:"center", justifyContent:"center", zIndex:999, backdropFilter:"blur(8px)" },
  modal: { background:"#111", border:"1px solid rgba(255,255,255,0.1)", borderRadius:12, padding:36, width:440, maxHeight:"90vh", overflowY:"auto" },
  input: { width:"100%", padding:"11px 14px", background:"rgba(255,255,255,0.04)", border:"1px solid rgba(255,255,255,0.1)", color:"#fff", borderRadius:6, fontSize:13, boxSizing:"border-box", display:"block", fontFamily:"'DM Sans', sans-serif", resize:"vertical" },
  toast: { position:"fixed", bottom:32, left:"50%", transform:"translateX(-50%)", background:"rgba(200,169,110,0.95)", color:"#0a0a0a", padding:"12px 28px", borderRadius:8, fontWeight:800, fontSize:13, zIndex:9999, letterSpacing:1, backdropFilter:"blur(10px)", whiteSpace:"nowrap" },
};

const CSS = `
  @import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;700;800;900&display=swap');
  * { box-sizing: border-box; }
  input::placeholder, textarea::placeholder { color: #444; }
  input:focus, textarea:focus { outline: none; border-color: rgba(200,169,110,0.4) !important; }
  input[type=range] { accent-color: #c8a96e; }
  select { appearance: none; }
  .hero-fade { animation: fadeIn 0.8s ease; }
  .hero-text { animation: slideUp 0.8s ease both; }
  .hero-img { animation: fadeIn 1s ease both; }
  .card-hover:hover { border-color: rgba(200,169,110,0.3) !important; }
  @keyframes fadeIn { from { opacity:0 } to { opacity:1 } }
  @keyframes slideUp { from { opacity:0; transform:translateY(20px) } to { opacity:1; transform:translateY(0) } }
  ::-webkit-scrollbar { width: 4px; }
  ::-webkit-scrollbar-track { background: #0a0a0a; }
  ::-webkit-scrollbar-thumb { background: #2a2a2a; border-radius: 2px; }
`;
