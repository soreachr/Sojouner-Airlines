import React from 'react';
import { BrowserRouter as Router, Routes, Route, Link, useNavigate, useParams } from 'react-router-dom';
import { motion } from 'framer-motion';

// Sojouner Airlines — Single-file React scaffold
// Assumes TailwindCSS + react-router-dom + framer-motion are installed.
// Drop into a Vite/CRA/Next app (client-side route file) and adjust as needed.

const FLIGHTS = [
  { id: 'SJ101', from: 'SFO', to: 'LAX', depart: '09:30', arrive: '10:50', duration: '1h20m', price: 89 },
  { id: 'SJ202', from: 'SFO', to: 'SEA', depart: '11:00', arrive: '13:10', duration: '2h10m', price: 129 },
  { id: 'SJ303', from: 'SFO', to: 'JFK', depart: '20:00', arrive: '04:30', duration: '5h30m', price: 349 },
];

function AppShell({ children }) {
  return (
    <div className="min-h-screen bg-slate-50 text-slate-800">
      <header className="bg-white shadow sticky top-0 z-30">
        <div className="max-w-7xl mx-auto px-4 py-4 flex items-center justify-between">
          <Link to="/" className="flex items-center gap-3">
            <div className="w-10 h-10 rounded-full bg-gradient-to-r from-emerald-400 to-sky-500 flex items-center justify-center text-white font-bold">SJ</div>
            <div>
              <div className="text-lg font-bold">Sojouner Airlines</div>
              <div className="text-xs text-gray-500">Fly calm. Fly Sojouner.</div>
            </div>
          </Link>

          <nav className="hidden md:flex items-center gap-4 text-sm">
            <Link to="/book" className="px-3 py-2 rounded hover:bg-slate-100">Book</Link>
            <Link to="/manage" className="px-3 py-2 rounded hover:bg-slate-100">Manage</Link>
            <Link to="/flight-status" className="px-3 py-2 rounded hover:bg-slate-100">Flight status</Link>
            <Link to="/fleet" className="px-3 py-2 rounded hover:bg-slate-100">Fleet</Link>
            <Link to="/about" className="px-3 py-2 rounded hover:bg-slate-100">About</Link>
            <Link to="/careers" className="px-3 py-2 rounded bg-emerald-50 text-emerald-600">Careers</Link>
          </nav>

          <div className="flex items-center gap-3">
            <Link to="/login" className="text-sm px-3 py-2 border rounded">Log in</Link>
            <MobileMenu />
          </div>
        </div>
      </header>

      <main className="max-w-7xl mx-auto px-4 py-8">{children}</main>

      <footer className="mt-12 py-8 bg-white border-t">
        <div className="max-w-7xl mx-auto px-4 text-sm text-gray-600 grid grid-cols-1 md:grid-cols-3 gap-4">
          <div>
            <div className="font-semibold">Sojouner Airlines</div>
            <div className="mt-2">© {new Date().getFullYear()} Sojouner Airlines · All rights reserved</div>
          </div>
          <div>
            <div className="font-semibold">Support</div>
            <ul className="mt-2 space-y-1">
              <li>Help center</li>
              <li>Contact us</li>
              <li>Accessibility</li>
            </ul>
          </div>
          <div>
            <div className="font-semibold">Legal</div>
            <ul className="mt-2 space-y-1">
              <li>Terms & Conditions</li>
              <li>Privacy policy</li>
            </ul>
          </div>
        </div>
      </footer>
    </div>
  );
}

function MobileMenu() {
  const [open, setOpen] = React.useState(false);
  return (
    <div className="md:hidden">
      <button onClick={() => setOpen(!open)} className="px-3 py-2 border rounded">
        Menu
      </button>
      {open && (
        <div className="absolute right-4 mt-2 bg-white rounded shadow p-3 w-48">
          <Link to="/book" className="block py-1">Book</Link>
          <Link to="/manage" className="block py-1">Manage</Link>
          <Link to="/flight-status" className="block py-1">Flight status</Link>
          <Link to="/fleet" className="block py-1">Fleet</Link>
          <Link to="/about" className="block py-1">About</Link>
        </div>
      )}
    </div>
  );
}

function Home() {
  return (
    <AppShell>
      <motion.section initial={{ opacity: 0 }} animate={{ opacity: 1 }} className="bg-white p-8 rounded-2xl shadow-sm">
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-6 items-center">
          <div>
            <h1 className="text-3xl font-bold">Welcome aboard Sojouner Airlines</h1>
            <p className="mt-3 text-gray-600">Comfort-focused flights connecting cities across the country. Simple fares, friendly crew, and a calmer travel experience.</p>

            <div className="mt-6">
              <Link to="/book" className="px-4 py-3 bg-sky-600 text-white rounded shadow">Book a flight</Link>
              <Link to="/fleet" className="ml-3 px-4 py-3 border rounded">Our fleet</Link>
            </div>

            <div className="mt-6 grid grid-cols-1 sm:grid-cols-3 gap-3">
              <Stat label="On-time" value="92%" />
              <Stat label="Destinations" value="65+" />
              <Stat label="Baggage" value="First bag free" />
            </div>
          </div>

          <div className="bg-gradient-to-br from-sky-50 to-emerald-50 p-6 rounded-lg">
            <BookingPreview />
          </div>
        </div>
      </motion.section>

      <section className="mt-8 grid grid-cols-1 md:grid-cols-3 gap-4">
        <FeatureCard title="Fly sustainable" body="We offset carbon on eligible routes and invest in sustainable aviation fuel research." />
        <FeatureCard title="Comfort-first" body="Extra legroom, quiet cabins, and curated snacks." />
        <FeatureCard title="Flexible fares" body="Change without penalty on most fares." />
      </section>

      <section className="mt-8">
        <h3 className="text-lg font-semibold">Popular routes</h3>
        <div className="mt-4 grid grid-cols-1 sm:grid-cols-3 gap-3">
          {FLIGHTS.map(f => (
            <Link key={f.id} to={`/flights/${f.id}`} className="block bg-white p-4 rounded shadow">
              <div className="flex justify-between items-center">
                <div>
                  <div className="font-medium">{f.from} → {f.to}</div>
                  <div className="text-xs text-gray-500">{f.duration} · {f.depart} — {f.arrive}</div>
                </div>
                <div className="text-right">
                  <div className="font-semibold">${f.price}</div>
                  <div className="text-xs text-gray-500">Economy</div>
                </div>
              </div>
            </Link>
          ))}
        </div>
      </section>
    </AppShell>
  );
}

function Stat({ label, value }) {
  return (
    <div className="bg-white p-4 rounded shadow text-center">
      <div className="text-2xl font-bold">{value}</div>
      <div className="text-xs text-gray-500">{label}</div>
    </div>
  );
}

function FeatureCard({ title, body }) {
  return (
    <div className="bg-white p-4 rounded shadow">
      <div className="font-semibold">{title}</div>
      <div className="text-sm text-gray-600 mt-2">{body}</div>
    </div>
  );
}

function BookingPreview() {
  const navigate = useNavigate();
  const [form, setForm] = React.useState({ from: 'SFO', to: 'LAX', date: '', passengers: 1 });
  return (
    <div>
      <div className="text-sm text-gray-500">Quick book</div>
      <div className="mt-3 space-y-3">
        <div className="grid grid-cols-2 gap-2">
          <select value={form.from} onChange={e => setForm({ ...form, from: e.target.value })} className="border rounded px-2 py-2">
            <option>SFO</option>
            <option>LAX</option>
            <option>SEA</option>
            <option>JFK</option>
          </select>
          <select value={form.to} onChange={e => setForm({ ...form, to: e.target.value })} className="border rounded px-2 py-2">
            <option>LAX</option>
            <option>SEA</option>
            <option>JFK</option>
            <option>SFO</option>
          </select>
        </div>
        <input type="date" value={form.date} onChange={e => setForm({ ...form, date: e.target.value })} className="border rounded px-2 py-2 w-full" />
        <div className="flex gap-2">
          <input type="number" min={1} value={form.passengers} onChange={e => setForm({ ...form, passengers: Number(e.target.value) })} className="border rounded px-2 py-2 w-24" />
          <button onClick={() => navigate('/book', { state: form })} className="px-3 py-2 bg-sky-600 text-white rounded">Search</button>
        </div>
      </div>
    </div>
  );
}

function Book() {
  const navigate = useNavigate();
  const [query, setQuery] = React.useState({ from: 'SFO', to: 'LAX', date: '', passengers: 1 });
  const [results, setResults] = React.useState([]);

  function doSearch(e) {
    e?.preventDefault();
    // Mock search — filter FLIGHTS
    const found = FLIGHTS.filter(f => f.from === query.from && f.to === query.to);
    setResults(found);
  }

  return (
    <AppShell>
      <div className="bg-white p-6 rounded shadow">
        <h2 className="text-xl font-semibold">Book a flight</h2>
        <form onSubmit={doSearch} className="mt-4 grid grid-cols-1 md:grid-cols-4 gap-3">
          <select value={query.from} onChange={e => setQuery({ ...query, from: e.target.value })} className="border rounded px-2 py-2">
            <option>SFO</option>
            <option>LAX</option>
            <option>SEA</option>
            <option>JFK</option>
          </select>
          <select value={query.to} onChange={e => setQuery({ ...query, to: e.target.value })} className="border rounded px-2 py-2">
            <option>LAX</option>
            <option>SEA</option>
            <option>JFK</option>
            <option>SFO</option>
          </select>
          <input type="date" value={query.date} onChange={e => setQuery({ ...query, date: e.target.value })} className="border rounded px-2 py-2" />
          <div className="flex gap-2">
            <input type="number" min={1} value={query.passengers} onChange={e => setQuery({ ...query, passengers: Number(e.target.value) })} className="border rounded px-2 py-2 w-24" />
            <button onClick={doSearch} className="px-3 py-2 bg-emerald-600 text-white rounded">Search</button>
          </div>
        </form>

        <div className="mt-6">
          {results.length === 0 ? (
            <div className="text-gray-500">No flights found. Try different dates or airports.</div>
          ) : (
            <div className="space-y-3">
              {results.map(r => (
                <div key={r.id} className="bg-slate-50 p-3 rounded flex items-center justify-between">
                  <div>
                    <div className="font-semibold">{r.id} · {r.from} → {r.to}</div>
                    <div className="text-xs text-gray-500">{r.depart} — {r.arrive} · {r.duration}</div>
                  </div>
                  <div className="text-right">
                    <div className="font-semibold">${r.price}</div>
                    <button onClick={() => navigate(`/flights/${r.id}`)} className="mt-2 px-3 py-2 border rounded">Select</button>
                  </div>
                </div>
              ))}
            </div>
          )}
        </div>
      </div>
    </AppShell>
  );
}

function FlightDetails() {
  const { id } = useParams();
  const flight = FLIGHTS.find(f => f.id === id);
  if (!flight) return (
    <AppShell>
      <div className="bg-white p-6 rounded">Flight not found.</div>
    </AppShell>
  );

  return (
    <AppShell>
      <div className="bg-white p-6 rounded shadow">
        <div className="flex justify-between items-start">
          <div>
            <h3 className="text-2xl font-bold">{flight.id}</h3>
            <div className="text-sm text-gray-500">{flight.from} → {flight.to}</div>
          </div>
          <div className="text-right">
            <div className="text-xl font-semibold">${flight.price}</div>
            <div className="text-xs text-gray-500">Economy</div>
          </div>
        </div>

        <div className="mt-6 grid grid-cols-1 md:grid-cols-3 gap-4">
          <div className="md:col-span-2">
            <h4 className="font-semibold">Summary</h4>
            <p className="text-sm text-gray-600 mt-2">Depart: {flight.depart} · Arrive: {flight.arrive} · Duration: {flight.duration}</p>

            <h4 className="font-semibold mt-4">What's included</h4>
            <ul className="mt-2 list-disc list-inside text-sm text-gray-600">
              <li>1 checked bag included on most fares</li>
              <li>Free in-flight Wi‑Fi on select routes</li>
              <li>Seat selection available</li>
            </ul>
          </div>

          <aside className="bg-slate-50 p-4 rounded">
            <div className="font-medium">Ready to book?</div>
            <div className="mt-3">
              <Link to="/book" className="px-3 py-2 bg-sky-600 text-white rounded">Book now</Link>
            </div>
          </aside>
        </div>
      </div>
    </AppShell>
  );
}

function Fleet() {
  const PLANE = [
    { model: 'A320neo', seats: 180, notes: 'Fuel-efficient single-aisle' },
    { model: 'B737 MAX', seats: 172, notes: 'Short/medium-haul workhorse' },
    { model: 'A321LR', seats: 200, notes: 'Transcontinental capable' },
  ];

  return (
    <AppShell>
      <div className="bg-white p-6 rounded shadow">
        <h2 className="text-xl font-semibold">Our fleet</h2>
        <div className="mt-4 grid grid-cols-1 md:grid-cols-3 gap-3">
          {PLANE.map(p => (
            <div key={p.model} className="p-4 border rounded">
              <div className="font-semibold">{p.model}</div>
              <div className="text-sm text-gray-500">Seats: {p.seats}</div>
              <div className="mt-2 text-sm">{p.notes}</div>
            </div>
          ))}
        </div>
      </div>
    </AppShell>
  );
}

function FlightStatus() {
  const [query, setQuery] = React.useState('');
  const [result, setResult] = React.useState(null);

  function check() {
    const f = FLIGHTS.find(f => f.id === query.toUpperCase());
    setResult(f || { notFound: true });
  }

  return (
    <AppShell>
      <div className="bg-white p-6 rounded shadow">
        <h2 className="text-lg font-semibold">Flight status</h2>
        <div className="mt-4 flex gap-2">
          <input value={query} onChange={e => setQuery(e.target.value)} placeholder="Enter flight number e.g. SJ101" className="border rounded px-3 py-2" />
          <button onClick={check} className="px-3 py-2 bg-sky-600 text-white rounded">Check</button>
        </div>

        {result && (
          <div className="mt-4">
            {result.notFound ? (
              <div className="text-gray-500">No information for that flight.</div>
            ) : (
              <div className="bg-slate-50 p-4 rounded">
                <div className="font-semibold">{result.id} · {result.from} → {result.to}</div>
                <div className="text-sm text-gray-500">Scheduled: {result.depart} — {result.arrive}</div>
                <div className="mt-2 text-sm">Status: On time</div>
              </div>
            )}
          </div>
        )}
      </div>
    </AppShell>
  );
}

function Manage() {
  return (
    <AppShell>
      <div className="bg-white p-6 rounded shadow">
        <h2 className="text-lg font-semibold">Manage booking</h2>
        <p className="mt-3 text-sm text-gray-500">Retrieve your booking by confirmation code and last name to change seats, add bags, or request special assistance.</p>
        <div className="mt-4 grid grid-cols-1 md:grid-cols-3 gap-3">
          <input placeholder="Confirmation code" className="border rounded px-2 py-2" />
          <input placeholder="Last name" className="border rounded px-2 py-2" />
          <button className="px-3 py-2 bg-emerald-600 text-white rounded">Find booking</button>
        </div>
      </div>
    </AppShell>
  );
}

function About() {
  return (
    <AppShell>
      <div className="bg-white p-6 rounded shadow">
        <h2 className="text-xl font-semibold">About Sojouner Airlines</h2>
        <p className="mt-3 text-gray-600">Sojouner Airlines was founded to make travel calmer and more sustainable. We focus on efficient operations, curated onboard experiences, and a kind crew.</p>

        <h4 className="font-semibold mt-4">Our mission</h4>
        <p className="text-sm text-gray-600 mt-2">Connect people while minimizing environmental impact and maximizing comfort.</p>
      </div>
    </AppShell>
  );
}

function Careers() {
  return (
    <AppShell>
      <div className="bg-white p-6 rounded shadow">
        <h2 className="text-xl font-semibold">Careers</h2>
        <p className="mt-3 text-gray-600">We're hiring for cabin crew, pilots, engineers, and ground operations. Join a values-driven team.</p>
        <div className="mt-4 space-y-3">
          <Job title="Cabin Crew" location="Multiple" />
          <Job title="Pilot — A320neo" location="SFO" />
          <Job title="Aircraft Mechanic" location="LAX" />
        </div>
      </div>
    </AppShell>
  );
}

function Job({ title, location }) {
  return (
    <div className="p-3 border rounded flex justify-between items-center">
      <div>
        <div className="font-semibold">{title}</div>
        <div className="text-sm text-gray-500">{location}</div>
      </div>
      <button className="px-3 py-2 bg-sky-600 text-white rounded">Apply</button>
    </div>
  );
}

function Login() {
  return (
    <AppShell>
      <div className="max-w-md mx-auto bg-white p-6 rounded shadow">
        <h3 className="text-lg font-semibold">Log in to your account</h3>
        <div className="mt-4 space-y-3">
          <input placeholder="Email" className="border rounded px-3 py-2 w-full" />
          <input placeholder="Password" type="password" className="border rounded px-3 py-2 w-full" />
          <button className="w-full px-3 py-2 bg-emerald-600 text-white rounded">Sign in</button>
        </div>
      </div>
    </AppShell>
  );
}

export default function SojounerAirlines() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/book" element={<Book />} />
        <Route path="/flights/:id" element={<FlightDetails />} />
        <Route path="/fleet" element={<Fleet />} />
        <Route path="/flight-status" element={<FlightStatus />} />
        <Route path="/manage" element={<Manage />} />
        <Route path="/about" element={<About />} />
        <Route path="/careers" element={<Careers />} />
        <Route path="/login" element={<Login />} />
        <Route path="*" element={<Home />} />
      </Routes>
    </Router>
  );
}

/*
Usage notes:
- This is a single-file React scaffold for a fictional airline called Sojouner Airlines.
- Recommended packages: react-router-dom, framer-motion, tailwindcss.
- To make production-ready: integrate with a bookings API, add authentication, PCI-compliant payments, accessibility checks, and performance optimizations.
- Want me to: export a full Vite project, create design mockups, write marketing copy for pages, or scaffold a backend API mock? Pick any and I'll do it next.
*/
