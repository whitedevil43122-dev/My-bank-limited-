import React, { useState, useEffect, useMemo, useCallback } from "react";
import {
  AreaChart,
  Area,
  XAxis,
  YAxis,
  Tooltip,
  ResponsiveContainer,
  CartesianGrid,
} from "recharts";
import {
  Landmark,
  Search,
  LayoutDashboard,
  LineChart as LineChartIcon,
  PiggyBank,
  CreditCard,
  Receipt,
  Bell,
  HelpCircle,
  Settings,
  Plus,
  X,
  Trash2,
  ArrowUpRight,
  ArrowDownRight,
  Wallet,
  Banknote,
  DollarSign,
  Wifi,
  Pencil,
  Utensils,
  ShoppingCart,
  Bus,
  Film,
  FileText,
  MoreHorizontal,
} from "lucide-react";

/* ============================== helpers ============================== */

const fmtMoney = (n) =>
  (n < 0 ? "-$" : "$") +
  Math.abs(n).toLocaleString(undefined, { minimumFractionDigits: 2, maximumFractionDigits: 2 });

const fmtPct = (n) => `${n >= 0 ? "+" : ""}${n.toFixed(2)}%`;

const daysBetween = (a, b) => (new Date(b) - new Date(a)) / (1000 * 60 * 60 * 24);
const addDays = (d, n) => {
  const dt = new Date(d);
  dt.setDate(dt.getDate() + n);
  return dt;
};
const isoDate = (d) => new Date(d).toISOString().slice(0, 10);
const uid = () => Math.random().toString(36).slice(2, 10);

function compoundValue(principal, annualRatePct, compoundsPerYear, years) {
  const r = annualRatePct / 100;
  if (compoundsPerYear <= 0) return principal * (1 + r * years);
  return principal * Math.pow(1 + r / compoundsPerYear, compoundsPerYear * years);
}
const FREQ_MAP = { Daily: 365, Monthly: 12, Quarterly: 4, Annually: 1 };

// Balance of an account at a given date. Savings accounts compound; everything
// else is a simple running ledger of principal + transactions.
function balanceAt(acc, atDate) {
  const txns = (acc.transactions || [])
    .filter((tx) => new Date(tx.date) <= atDate)
    .sort((a, b) => new Date(a.date) - new Date(b.date));

  if (acc.type !== "Savings") {
    return acc.principal + txns.reduce((s, t) => s + t.amount, 0);
  }

  let balance = acc.principal;
  let lastDate = acc.createdDate;
  for (const tx of txns) {
    const years = daysBetween(lastDate, tx.date) / 365;
    if (years > 0) balance = compoundValue(balance, acc.rate, FREQ_MAP[acc.freq], years);
    balance += tx.amount;
    lastDate = tx.date;
  }
  const remainingYears = daysBetween(lastDate, atDate) / 365;
  if (remainingYears > 0) balance = compoundValue(balance, acc.rate, FREQ_MAP[acc.freq], remainingYears);
  return balance;
}

function interestEarnedAt(acc, atDate) {
  if (acc.type !== "Savings") return 0;
  const netDeposits = (acc.transactions || [])
    .filter((tx) => new Date(tx.date) <= atDate)
    .reduce((s, t) => s + t.amount, 0);
  return balanceAt(acc, atDate) - acc.principal - netDeposits;
}

/* ---------- account theming ---------- */
const ACCOUNT_META = {
  Payout: { color: "#2E90FA", bg: "#EAF2FF", icon: Wallet, label: "Payout" },
  USD: { color: "#12B76A", bg: "#ECFDF3", icon: DollarSign, label: "USD" },
  Cash: { color: "#F79009", bg: "#FFF6E9", icon: Banknote, label: "Cash" },
  Savings: { color: "#7A5AF8", bg: "#F3EFFF", icon: PiggyBank, label: "Savings" },
};

const CATEGORY_META = {
  Food: { color: "#F97066", icon: Utensils },
  Grocery: { color: "#12B76A", icon: ShoppingCart },
  Transport: { color: "#2E90FA", icon: Bus },
  Entertainment: { color: "#7A5AF8", icon: Film },
  Bills: { color: "#667085", icon: FileText },
  Other: { color: "#98A2B3", icon: MoreHorizontal },
};

const CARD_THEMES = [
  { from: "#0B1E3F", to: "#12305F", accent: "#5B9BFF" },
  { from: "#0F3D3E", to: "#116466", accent: "#4FD1C5" },
  { from: "#3A1249", to: "#5B1E6E", accent: "#C084FC" },
];

/* ---------- storage ---------- */
async function loadKey(key, fallback) {
  try {
    const res = await window.storage.get(key, false);
    return res ? JSON.parse(res.value) : fallback;
  } catch (e) {
    return fallback;
  }
}
async function saveKey(key, value) {
  try {
    await window.storage.set(key, JSON.stringify(value), false);
  } catch (e) {
    console.error("storage error", e);
  }
}

function seedAccounts() {
  const today = isoDate(new Date());
  return [
    { id: uid(), type: "Payout", principal: 0, createdDate: today, transactions: [] },
    { id: uid(), type: "USD", principal: 0, createdDate: today, transactions: [] },
    { id: uid(), type: "Cash", principal: 0, createdDate: today, transactions: [] },
    { id: uid(), type: "Savings", principal: 0, rate: 4.5, freq: "Monthly", createdDate: today, transactions: [] },
  ];
}

function seedCards() {
  return [
    { id: uid(), label: "Everyday", number: "", theme: 0, transactions: [] },
    { id: uid(), label: "Travel", number: "", theme: 1, transactions: [] },
    { id: uid(), label: "Business", number: "", theme: 2, transactions: [] },
  ];
}

/* ============================== UI atoms ============================== */

function Panel({ children, className = "" }) {
  return (
    <div className={`rounded-2xl bg-white border border-[#EAECF0] shadow-sm p-6 ${className}`}>
      {children}
    </div>
  );
}

function Pill({ children, tone = "neutral" }) {
  const tones = {
    up: "bg-[#ECFDF3] text-[#12B76A]",
    down: "bg-[#FEF3F2] text-[#F04438]",
    neutral: "bg-[#F2F4F7] text-[#475467]",
  };
  return (
    <span className={`inline-flex items-center gap-1 rounded-full px-2 py-0.5 text-xs font-semibold ${tones[tone]}`}>
      {children}
    </span>
  );
}

function Button({ children, onClick, variant = "primary", type = "button", className = "" }) {
  const base = "inline-flex items-center gap-1.5 rounded-lg px-3.5 py-2 text-sm font-medium transition-colors duration-150";
  const variants = {
    primary: "bg-[#0B5FFF] text-white hover:bg-[#0A4FDB]",
    ghost: "bg-transparent border border-[#D0D5DD] text-[#344054] hover:border-[#0B5FFF] hover:text-[#0B5FFF]",
    danger: "bg-transparent text-[#F04438] hover:bg-[#FEF3F2]",
  };
  return (
    <button type={type} onClick={onClick} className={`${base} ${variants[variant]} ${className}`}>
      {children}
    </button>
  );
}

function Input({ label, ...props }) {
  return (
    <label className="flex flex-col gap-1 text-xs text-[#667085]">
      {label}
      <input
        {...props}
        className="rounded-lg bg-white border border-[#D0D5DD] px-3 py-2 text-sm text-[#101828] outline-none focus:border-[#0B5FFF] focus:ring-1 focus:ring-[#0B5FFF]/30"
      />
    </label>
  );
}

function Select({ label, options, ...props }) {
  return (
    <label className="flex flex-col gap-1 text-xs text-[#667085]">
      {label}
      <select
        {...props}
        className="rounded-lg bg-white border border-[#D0D5DD] px-3 py-2 text-sm text-[#101828] outline-none focus:border-[#0B5FFF] focus:ring-1 focus:ring-[#0B5FFF]/30"
      >
        {options.map((o) => (
          <option key={o} value={o}>{o}</option>
        ))}
      </select>
    </label>
  );
}

function CustomTooltip({ active, payload, label }) {
  if (!active || !payload || !payload.length) return null;
  return (
    <div className="rounded-xl bg-white border border-[#EAECF0] shadow-lg px-3.5 py-2.5">
      <div className="flex items-center gap-1.5 text-sm font-semibold text-[#101828]">
        <span className="w-2 h-2 rounded-full bg-[#12B76A]" />
        {fmtMoney(payload[0].value)} balance
      </div>
      <div className="text-xs text-[#98A2B3] mt-0.5">{label}</div>
    </div>
  );
}

/* ---------- balance history / chart series ---------- */

const PERIODS = ["1D", "7D", "1M", "3M", "ALL"];

function earliestDate(accounts) {
  let min = new Date();
  accounts.forEach((a) => {
    if (new Date(a.createdDate) < min) min = new Date(a.createdDate);
  });
  return min;
}

function buildSeries(accounts, period) {
  const now = new Date();
  let start;
  let points;
  let labelFmt;

  if (period === "1D") {
    start = addDays(now, -1);
    points = 12;
    labelFmt = (d) => d.toLocaleTimeString(undefined, { hour: "numeric" });
  } else if (period === "7D") {
    start = addDays(now, -7);
    points = 7;
    labelFmt = (d) => d.toLocaleDateString(undefined, { weekday: "short" });
  } else if (period === "1M") {
    start = addDays(now, -30);
    points = 15;
    labelFmt = (d) => d.toLocaleDateString(undefined, { month: "short", day: "numeric" });
  } else if (period === "3M") {
    start = addDays(now, -90);
    points = 13;
    labelFmt = (d) => d.toLocaleDateString(undefined, { month: "short", day: "numeric" });
  } else {
    start = earliestDate(accounts);
    if (now - start < 1000 * 60 * 60 * 24) start = addDays(now, -30);
    points = 14;
    labelFmt = (d) => d.toLocaleDateString(undefined, { month: "short", year: "2-digit" });
  }

  const step = (now - start) / (points - 1);
  const data = [];
  for (let i = 0; i < points; i++) {
    const d = new Date(start.getTime() + step * i);
    const total = accounts.reduce((s, a) => s + balanceAt(a, d), 0);
    data.push({ label: labelFmt(d), value: Math.max(total, 0) });
  }
  return data;
}

function BalanceChart({ accounts, height = 260 }) {
  const [period, setPeriod] = useState("7D");
  const data = useMemo(() => buildSeries(accounts, period), [accounts, period]);
  const hasData = accounts.some((a) => (a.transactions || []).length > 0);

  return (
    <div>
      <div className="flex items-center justify-between mb-2">
        <h3 className="text-base font-semibold text-[#101828]">Statistics</h3>
        <div className="flex items-center bg-[#F2F4F7] rounded-lg p-0.5">
          {PERIODS.map((p) => (
            <button
              key={p}
              onClick={() => setPeriod(p)}
              className={`px-2.5 py-1 text-xs font-medium rounded-md transition-colors ${
                period === p ? "bg-white text-[#101828] shadow-sm" : "text-[#667085] hover:text-[#101828]"
              }`}
            >
              {p}
            </button>
          ))}
        </div>
      </div>
      <div style={{ height }} className="-mx-2">
        {hasData ? (
          <ResponsiveContainer width="100%" height="100%">
            <AreaChart data={data} margin={{ top: 10, right: 10, left: 0, bottom: 0 }}>
              <defs>
                <linearGradient id="balGrad" x1="0" y1="0" x2="0" y2="1">
                  <stop offset="0%" stopColor="#12B76A" stopOpacity={0.35} />
                  <stop offset="100%" stopColor="#12B76A" stopOpacity={0} />
                </linearGradient>
              </defs>
              <CartesianGrid stroke="#EEF1F5" strokeDasharray="3 3" vertical={false} />
              <XAxis dataKey="label" tick={{ fill: "#98A2B3", fontSize: 11 }} axisLine={{ stroke: "#EAECF0" }} tickLine={false} />
              <YAxis
                tick={{ fill: "#98A2B3", fontSize: 11 }}
                axisLine={false}
                tickLine={false}
                tickFormatter={(v) => `$${v >= 1000 ? (v / 1000).toFixed(1) + "k" : v}`}
              />
              <Tooltip content={<CustomTooltip />} />
              <Area type="monotone" dataKey="value" stroke="#12B76A" strokeWidth={2.5} fill="url(#balGrad)" />
            </AreaChart>
          </ResponsiveContainer>
        ) : (
          <div className="h-full flex items-center justify-center text-sm text-[#98A2B3]">
            No activity yet — add a transaction to see your balance history.
          </div>
        )}
      </div>
    </div>
  );
}

/* ============================== Sidebar ============================== */

function NavItem({ icon: Icon, label, active, onClick }) {
  return (
    <button
      onClick={onClick}
      className={`w-full flex items-center gap-3 rounded-lg px-3 py-2.5 text-sm font-medium transition-colors ${
        active ? "bg-[#EAF2FF] text-[#0B5FFF]" : "text-[#475467] hover:bg-[#F9FAFB]"
      }`}
    >
      <Icon size={18} className={active ? "text-[#0B5FFF]" : "text-[#98A2B3]"} />
      {label}
    </button>
  );
}

function Sidebar({ page, setPage }) {
  const [search, setSearch] = useState("");
  const nav = [
    { key: "dashboard", label: "Dashboard", icon: LayoutDashboard },
    { key: "finance", label: "Finance", icon: LineChartIcon },
    { key: "accounts", label: "Accounts", icon: PiggyBank },
    { key: "cards", label: "Cards", icon: CreditCard },
    { key: "expenses", label: "Expenses", icon: Receipt },
  ];
  return (
    <aside className="w-72 shrink-0 bg-white border-r border-[#EAECF0] flex flex-col h-screen sticky top-0">
      <div className="p-6 pb-4">
        <div className="flex items-center gap-2">
          <div className="w-8 h-8 rounded-lg bg-[#0B5FFF] flex items-center justify-center">
            <Landmark size={18} className="text-white" />
          </div>
          <span className="text-lg font-bold text-[#101828] tracking-tight">MK Bank Ltd</span>
        </div>
      </div>

      <div className="px-6 mb-4">
        <div className="flex items-center gap-2 rounded-lg border border-[#D0D5DD] px-3 py-2 text-sm text-[#98A2B3]">
          <Search size={15} />
          <input
            value={search}
            onChange={(e) => setSearch(e.target.value)}
            placeholder="Search..."
            className="bg-transparent outline-none flex-1 text-[#101828] placeholder:text-[#98A2B3]"
          />
        </div>
      </div>

      <nav className="px-4 flex flex-col gap-1">
        {nav.map((n) => (
          <NavItem key={n.key} icon={n.icon} label={n.label} active={page === n.key} onClick={() => setPage(n.key)} />
        ))}
      </nav>

      <div className="mt-auto px-4 pb-5 flex flex-col gap-1">
        <NavItem icon={Bell} label="Notifications" active={false} onClick={() => {}} />
        <NavItem icon={HelpCircle} label="Help Center" active={false} onClick={() => {}} />
        <NavItem icon={Settings} label="Settings" active={false} onClick={() => {}} />
        <div className="mt-3 flex items-center gap-3 rounded-xl border border-[#EAECF0] px-3 py-2.5">
          <div className="w-9 h-9 rounded-full bg-[#0B5FFF] text-white flex items-center justify-center text-sm font-semibold">
            Mk
          </div>
          <div className="min-w-0">
            <div className="text-sm font-medium text-[#101828] truncate">Mk</div>
            <div className="text-xs text-[#98A2B3] truncate">mk@mkbank.ltd</div>
          </div>
        </div>
      </div>
    </aside>
  );
}

/* ============================== Dashboard ============================== */

function TransactionRow({ tx, accountLabel, accentColor }) {
  const positive = tx.amount >= 0;
  return (
    <div className="flex items-center justify-between py-3 border-b border-[#F2F4F7] last:border-0">
      <div className="flex items-center gap-3 min-w-0">
        <div
          className="w-9 h-9 rounded-full flex items-center justify-center shrink-0"
          style={{ background: positive ? "#ECFDF3" : "#FEF3F2" }}
        >
          {positive ? <ArrowDownRight size={16} className="text-[#12B76A]" /> : <ArrowUpRight size={16} className="text-[#F04438]" />}
        </div>
        <div className="min-w-0">
          <div className="text-sm font-medium text-[#101828] truncate">
            {positive ? "Deposit" : "Withdrawal"} · {accountLabel}
          </div>
          <div className="text-xs text-[#98A2B3]">{tx.date}{tx.note ? ` · ${tx.note}` : ""}</div>
        </div>
      </div>
      <div className={`text-sm font-semibold shrink-0 ${positive ? "text-[#12B76A]" : "text-[#F04438]"}`}>
        {positive ? "+" : "-"}{fmtMoney(Math.abs(tx.amount))}
      </div>
    </div>
  );
}

function allTransactions(accounts) {
  const rows = [];
  accounts.forEach((a) => {
    (a.transactions || []).forEach((tx) =>
      rows.push({ ...tx, accountLabel: ACCOUNT_META[a.type].label, accountType: a.type })
    );
  });
  return rows.sort((a, b) => new Date(b.date) - new Date(a.date));
}

function Dashboard({ accounts }) {
  const total = useMemo(() => accounts.reduce((s, a) => s + balanceAt(a, new Date()), 0), [accounts]);
  const monthAgoTotal = useMemo(
    () => accounts.reduce((s, a) => s + balanceAt(a, addDays(new Date(), -30)), 0),
    [accounts]
  );
  const pctChange = monthAgoTotal > 0 ? ((total - monthAgoTotal) / monthAgoTotal) * 100 : 0;
  const txns = useMemo(() => allTransactions(accounts).slice(0, 8), [accounts]);

  return (
    <div className="max-w-5xl">
      <h1 className="text-2xl font-bold text-[#101828] mb-6">Dashboard</h1>

      <Panel className="mb-6">
        <div className="text-xs uppercase tracking-wide text-[#98A2B3] font-medium mb-1">My Balance</div>
        <div className="flex items-center gap-3 mb-4">
          <span className="text-4xl font-bold text-[#101828]">{fmtMoney(total)}</span>
          <Pill tone={pctChange >= 0 ? "up" : "down"}>{fmtPct(pctChange)}</Pill>
        </div>
        <BalanceChart accounts={accounts} />
      </Panel>

      <Panel>
        <h2 className="text-base font-semibold text-[#101828] mb-1">Recent Transactions</h2>
        {txns.length === 0 ? (
          <div className="text-sm text-[#98A2B3] py-8 text-center">No transactions yet.</div>
        ) : (
          <div className="mt-2">
            {txns.map((tx) => (
              <TransactionRow key={tx.id} tx={tx} accountLabel={tx.accountLabel} />
            ))}
          </div>
        )}
      </Panel>
    </div>
  );
}

/* ============================== Finance ============================== */

function PortfolioStats({ accounts }) {
  const total = accounts.reduce((s, a) => s + balanceAt(a, new Date()), 0);
  return (
    <Panel>
      <div className="flex items-center justify-between mb-4">
        <h2 className="text-base font-semibold text-[#101828]">Portfolio Stats</h2>
        <span className="text-xs font-medium text-[#0B5FFF] cursor-pointer">View All +</span>
      </div>
      <div className="flex flex-col gap-3">
        {accounts.map((a) => {
          const meta = ACCOUNT_META[a.type];
          const Icon = meta.icon;
          const bal = balanceAt(a, new Date());
          const pct = total > 0 ? (bal / total) * 100 : 0;
          return (
            <div key={a.id} className="flex items-center justify-between">
              <div className="flex items-center gap-2.5">
                <div className="w-8 h-8 rounded-full flex items-center justify-center" style={{ background: meta.bg }}>
                  <Icon size={15} style={{ color: meta.color }} />
                </div>
                <div>
                  <div className="text-sm font-medium text-[#101828]">{meta.label}</div>
                  <div className="text-xs text-[#98A2B3]">{pct.toFixed(2)}%</div>
                </div>
              </div>
              <div className="text-sm font-semibold text-[#101828]">{fmtMoney(bal)}</div>
            </div>
          );
        })}
      </div>
    </Panel>
  );
}

function InterestEarnedPanel({ accounts }) {
  const savings = accounts.filter((a) => a.type === "Savings");
  const now = new Date();
  const startOfMonth = new Date(now.getFullYear(), now.getMonth(), 1);
  const startOfLastMonth = new Date(now.getFullYear(), now.getMonth() - 1, 1);

  const totalInterest = savings.reduce((s, a) => s + interestEarnedAt(a, now), 0);
  const totalPrincipal = savings.reduce((s, a) => s + a.principal, 0);
  const thisMonth = savings.reduce((s, a) => s + (interestEarnedAt(a, now) - interestEarnedAt(a, startOfMonth)), 0);
  const lastMonth = savings.reduce(
    (s, a) => s + (interestEarnedAt(a, startOfMonth) - interestEarnedAt(a, startOfLastMonth)),
    0
  );
  const yieldPct = totalPrincipal > 0 ? (totalInterest / totalPrincipal) * 100 : 0;
  const gaugePct = Math.min(Math.max((yieldPct / 20) * 100, 2), 100);

  return (
    <Panel>
      <h2 className="text-base font-semibold text-[#101828] mb-4">Interest Earned</h2>
      <div className="flex items-end gap-2 mb-1">
        <span className="text-3xl font-bold text-[#101828]">{fmtMoney(Math.max(totalInterest, 0))}</span>
      </div>
      <div className="text-xs text-[#98A2B3] mb-4">total return on savings</div>

      <div className="relative h-2 rounded-full mb-2" style={{ background: "linear-gradient(90deg,#F04438,#F79009,#12B76A)" }}>
        <div
          className="absolute -top-1 w-4 h-4 rounded-full bg-white border-2 border-[#101828] shadow"
          style={{ left: `calc(${gaugePct}% - 8px)` }}
        />
      </div>
      <div className="flex justify-between text-[11px] text-[#98A2B3] mb-4">
        <span>0%</span>
        <span>{yieldPct.toFixed(2)}% yield</span>
        <span>20%</span>
      </div>

      <div className="flex items-center gap-2">
        <Pill tone={thisMonth >= 0 ? "up" : "down"}>This month {fmtMoney(thisMonth)}</Pill>
        <Pill tone={lastMonth >= 0 ? "up" : "down"}>Last month {fmtMoney(lastMonth)}</Pill>
      </div>
    </Panel>
  );
}

function TransactionsTable({ accounts }) {
  const [query, setQuery] = useState("");
  const all = allTransactions(accounts);
  const txns = query
    ? all.filter(
        (tx) =>
          tx.accountLabel.toLowerCase().includes(query.toLowerCase()) ||
          (tx.note || "").toLowerCase().includes(query.toLowerCase())
      )
    : all;
  return (
    <Panel>
      <div className="flex items-center justify-between mb-4 gap-3 flex-wrap">
        <h2 className="text-base font-semibold text-[#101828]">Transactions</h2>
        <div className="flex items-center gap-2">
          <div className="flex items-center gap-2 rounded-lg border border-[#D0D5DD] px-3 py-1.5 text-sm text-[#98A2B3]">
            <Search size={14} />
            <input
              value={query}
              onChange={(e) => setQuery(e.target.value)}
              placeholder="Search transactions..."
              className="bg-transparent outline-none text-[#101828] placeholder:text-[#98A2B3] w-40"
            />
          </div>
          <Button variant="ghost" className="text-xs py-1.5">
            Filter
          </Button>
        </div>
      </div>
      {txns.length === 0 ? (
        <div className="text-sm text-[#98A2B3] py-8 text-center">No transactions yet.</div>
      ) : (
        <div className="overflow-x-auto">
          <table className="w-full text-sm">
            <thead>
              <tr className="text-left text-[11px] uppercase tracking-wide text-[#98A2B3] border-b border-[#EAECF0]">
                <th className="pb-2 font-medium">Account</th>
                <th className="pb-2 font-medium">Date</th>
                <th className="pb-2 font-medium">Note</th>
                <th className="pb-2 font-medium text-right">Amount</th>
              </tr>
            </thead>
            <tbody>
              {txns.map((tx) => (
                <tr key={tx.id} className="border-b border-[#F2F4F7] last:border-0">
                  <td className="py-2.5 text-[#101828] font-medium">{tx.accountLabel}</td>
                  <td className="py-2.5 text-[#667085]">{tx.date}</td>
                  <td className="py-2.5 text-[#667085]">{tx.note || "—"}</td>
                  <td className={`py-2.5 text-right font-semibold ${tx.amount >= 0 ? "text-[#12B76A]" : "text-[#F04438]"}`}>
                    {tx.amount >= 0 ? "+" : "-"}{fmtMoney(Math.abs(tx.amount))}
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      )}
    </Panel>
  );
}

function Finance({ accounts }) {
  const [selected, setSelected] = useState("All");
  const now = new Date();
  const monthAgo = addDays(now, -30);

  const chartAccounts = selected === "All" ? accounts : accounts.filter((a) => a.type === selected);
  const total = chartAccounts.reduce((s, a) => s + balanceAt(a, now), 0);
  const monthAgoTotal = chartAccounts.reduce((s, a) => s + balanceAt(a, monthAgo), 0);
  const pctChange = monthAgoTotal > 0 ? ((total - monthAgoTotal) / monthAgoTotal) * 100 : 0;

  return (
    <div>
      <h1 className="text-2xl font-bold text-[#101828] mb-6">Finance</h1>

      {/* Row 1: chart (left, wide) + stacked stats (right) — mirrors the reference layout */}
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-6 items-start">
        <Panel className="lg:col-span-2">
          <div className="flex items-start justify-between mb-1">
            <div>
              <div className="text-xs uppercase tracking-wide text-[#98A2B3] font-medium mb-1">My Portfolio</div>
              <div className="flex items-center gap-3">
                <span className="text-3xl font-bold text-[#101828]">{fmtMoney(total)}</span>
                <Pill tone={pctChange >= 0 ? "up" : "down"}>{fmtPct(pctChange)}</Pill>
              </div>
            </div>
            <select
              value={selected}
              onChange={(e) => setSelected(e.target.value)}
              className="rounded-lg border border-[#D0D5DD] px-3 py-2 text-sm text-[#101828] outline-none focus:border-[#0B5FFF] bg-white"
            >
              <option value="All">All accounts</option>
              {accounts.map((a) => (
                <option key={a.id} value={a.type}>{ACCOUNT_META[a.type].label}</option>
              ))}
            </select>
          </div>
          <div className="mt-4">
            <BalanceChart accounts={chartAccounts} height={280} />
          </div>
        </Panel>

        <div className="flex flex-col gap-6">
          <PortfolioStats accounts={accounts} />
          <InterestEarnedPanel accounts={accounts} />
        </div>
      </div>

      {/* Row 2: full-width transactions, same as the Crypto Market panel position in the reference */}
      <TransactionsTable accounts={accounts} />
    </div>
  );
}

/* ============================== Accounts ============================== */

function TxnForm({ onSubmit, onClose }) {
  const [type, setType] = useState("Deposit");
  const [amount, setAmount] = useState("");
  const [date, setDate] = useState(isoDate(new Date()));
  const [note, setNote] = useState("");

  const submit = (e) => {
    e.preventDefault();
    if (!amount) return;
    const val = parseFloat(amount);
    onSubmit({ id: uid(), date, note, amount: type === "Withdraw" ? -Math.abs(val) : Math.abs(val) });
    onClose();
  };

  return (
    <form onSubmit={submit} className="grid grid-cols-2 gap-3 mt-3 pt-3 border-t border-[#F2F4F7]">
      <Select label="Type" options={["Deposit", "Withdraw"]} value={type} onChange={(e) => setType(e.target.value)} />
      <Input label="Amount ($)" type="number" step="0.01" placeholder="250" value={amount} onChange={(e) => setAmount(e.target.value)} />
      <Input label="Date" type="date" value={date} onChange={(e) => setDate(e.target.value)} />
      <Input label="Note (optional)" placeholder="Paycheck" value={note} onChange={(e) => setNote(e.target.value)} />
      <div className="col-span-2 flex justify-end gap-2 pt-1">
        <Button variant="ghost" onClick={onClose}>Cancel</Button>
        <Button type="submit">Save transaction</Button>
      </div>
    </form>
  );
}

function AccountCard({ acc, onAddTransaction, onDeleteTransaction }) {
  const [showForm, setShowForm] = useState(false);
  const [showHistory, setShowHistory] = useState(false);
  const meta = ACCOUNT_META[acc.type];
  const Icon = meta.icon;
  const bal = balanceAt(acc, new Date());
  const interest = acc.type === "Savings" ? interestEarnedAt(acc, new Date()) : 0;
  const txns = [...(acc.transactions || [])].sort((a, b) => new Date(b.date) - new Date(a.date));

  return (
    <Panel>
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-3">
          <div className="w-10 h-10 rounded-full flex items-center justify-center" style={{ background: meta.bg }}>
            <Icon size={18} style={{ color: meta.color }} />
          </div>
          <div>
            <div className="text-sm font-semibold text-[#101828]">{meta.label}</div>
            {acc.type === "Savings" && (
              <div className="text-xs text-[#98A2B3]">{acc.rate}% APY · {acc.freq.toLowerCase()}</div>
            )}
          </div>
        </div>
        <div className="text-right">
          <div className="text-lg font-bold text-[#101828]">{fmtMoney(bal)}</div>
          {acc.type === "Savings" && <div className="text-xs text-[#12B76A] font-medium">+{fmtMoney(Math.max(interest, 0))} interest</div>}
        </div>
      </div>

      <div className="flex items-center gap-2 mt-4">
        <Button variant="ghost" className="text-xs" onClick={() => setShowForm((v) => !v)}>
          {showForm ? <X size={13} /> : <Plus size={13} />} {showForm ? "Close" : "Add transaction"}
        </Button>
        {txns.length > 0 && (
          <button onClick={() => setShowHistory((v) => !v)} className="text-xs text-[#98A2B3] hover:text-[#475467] ml-auto">
            {showHistory ? "Hide" : "Show"} {txns.length} transaction{txns.length !== 1 ? "s" : ""}
          </button>
        )}
      </div>

      {showForm && <TxnForm onSubmit={(tx) => onAddTransaction(acc.id, tx)} onClose={() => setShowForm(false)} />}

      {showHistory && txns.length > 0 && (
        <div className="mt-3 pt-3 border-t border-[#F2F4F7] space-y-1">
          {txns.map((tx) => (
            <div key={tx.id} className="flex items-center justify-between text-xs py-1.5">
              <span className="text-[#667085]">{tx.date}{tx.note ? ` · ${tx.note}` : ""}</span>
              <span className="flex items-center gap-2">
                <span className={tx.amount < 0 ? "text-[#F04438]" : "text-[#12B76A]"}>
                  {tx.amount < 0 ? "-" : "+"}{fmtMoney(Math.abs(tx.amount))}
                </span>
                <button onClick={() => onDeleteTransaction(acc.id, tx.id)} className="text-[#98A2B3] hover:text-[#F04438]">
                  <X size={12} />
                </button>
              </span>
            </div>
          ))}
        </div>
      )}
    </Panel>
  );
}

function Accounts({ accounts, addTransaction, deleteTransaction }) {
  return (
    <div className="max-w-4xl">
      <h1 className="text-2xl font-bold text-[#101828] mb-1">Accounts</h1>
      <p className="text-sm text-[#98A2B3] mb-6">Log deposits and withdrawals across your four accounts.</p>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-5">
        {accounts.map((acc) => (
          <AccountCard key={acc.id} acc={acc} onAddTransaction={addTransaction} onDeleteTransaction={deleteTransaction} />
        ))}
      </div>
    </div>
  );
}

/* ============================== Cards ============================== */

function maskNumber(num) {
  const digits = (num || "").replace(/\D/g, "").padEnd(16, "•");
  const groups = digits.match(/.{1,4}/g) || [];
  return groups.join("  ");
}

function BankCard({ card, onChangeNumber }) {
  const [editing, setEditing] = useState(false);
  const [draft, setDraft] = useState(card.number);
  const theme = CARD_THEMES[card.theme % CARD_THEMES.length];

  const save = () => {
    onChangeNumber(card.id, draft.replace(/\D/g, "").slice(0, 16));
    setEditing(false);
  };

  return (
    <div
      className="rounded-2xl p-5 text-white shadow-lg relative overflow-hidden h-48 flex flex-col justify-between"
      style={{ background: `linear-gradient(135deg, ${theme.from}, ${theme.to})` }}
    >
      <div
        className="pointer-events-none absolute -right-10 -top-10 w-40 h-40 rounded-full opacity-20"
        style={{ background: theme.accent }}
      />
      <div className="flex items-center justify-between relative">
        <span className="text-sm font-semibold tracking-wide">{card.label}</span>
        <Wifi size={18} className="rotate-90 opacity-80" />
      </div>

      <div className="relative">
        <div className="w-9 h-7 rounded-md mb-3" style={{ background: `linear-gradient(135deg, ${theme.accent}, #fff8)` }} />
        {editing ? (
          <div className="flex items-center gap-2">
            <input
              autoFocus
              value={draft}
              onChange={(e) => setDraft(e.target.value)}
              maxLength={19}
              placeholder="Enter card number"
              className="bg-white/15 border border-white/30 rounded-md px-2 py-1 text-sm font-mono tracking-widest outline-none placeholder:text-white/50 w-full"
            />
            <button onClick={save} className="text-xs bg-white text-[#101828] rounded-md px-2 py-1 font-medium">Save</button>
          </div>
        ) : (
          <button onClick={() => setEditing(true)} className="flex items-center gap-2 font-mono text-lg tracking-widest group">
            {maskNumber(card.number)}
            <Pencil size={13} className="opacity-0 group-hover:opacity-70 transition-opacity" />
          </button>
        )}
      </div>

      <div className="flex items-center justify-between relative text-xs">
        <div>
          <div className="opacity-60 text-[10px] uppercase">Card holder</div>
          <div className="font-medium">Mk</div>
        </div>
        <div>
          <div className="opacity-60 text-[10px] uppercase">MK Bank Ltd</div>
        </div>
      </div>
    </div>
  );
}

function CardTxnForm({ onSubmit, onClose }) {
  const [amount, setAmount] = useState("");
  const [category, setCategory] = useState("Food");
  const [date, setDate] = useState(isoDate(new Date()));
  const [note, setNote] = useState("");

  const submit = (e) => {
    e.preventDefault();
    if (!amount) return;
    onSubmit({ id: uid(), date, note, category, amount: -Math.abs(parseFloat(amount)) });
    onClose();
  };

  return (
    <form onSubmit={submit} className="grid grid-cols-2 gap-3 mt-3">
      <Input label="Amount ($)" type="number" step="0.01" placeholder="42.50" value={amount} onChange={(e) => setAmount(e.target.value)} />
      <Select label="Category" options={Object.keys(CATEGORY_META)} value={category} onChange={(e) => setCategory(e.target.value)} />
      <Input label="Date" type="date" value={date} onChange={(e) => setDate(e.target.value)} />
      <Input label="Note (optional)" placeholder="Coffee with client" value={note} onChange={(e) => setNote(e.target.value)} />
      <div className="col-span-2 flex justify-end gap-2 pt-1">
        <Button variant="ghost" onClick={onClose}>Cancel</Button>
        <Button type="submit">Add expense</Button>
      </div>
    </form>
  );
}

function Cards({ cards, changeCardNumber, addCardTransaction, deleteCardTransaction }) {
  const [openFormId, setOpenFormId] = useState(null);

  return (
    <div className="max-w-5xl">
      <h1 className="text-2xl font-bold text-[#101828] mb-1">Cards</h1>
      <p className="text-sm text-[#98A2B3] mb-6">Tap a card number to edit it, then log expenses against each card.</p>

      <div className="grid grid-cols-1 md:grid-cols-3 gap-5 mb-8">
        {cards.map((c) => (
          <BankCard key={c.id} card={c} onChangeNumber={changeCardNumber} />
        ))}
      </div>

      <div className="grid grid-cols-1 md:grid-cols-3 gap-5">
        {cards.map((c) => {
          const txns = [...(c.transactions || [])].sort((a, b) => new Date(b.date) - new Date(a.date));
          return (
            <Panel key={c.id}>
              <div className="flex items-center justify-between mb-2">
                <h3 className="text-sm font-semibold text-[#101828]">{c.label} card</h3>
                <Button variant="ghost" className="text-xs" onClick={() => setOpenFormId(openFormId === c.id ? null : c.id)}>
                  {openFormId === c.id ? <X size={13} /> : <Plus size={13} />} {openFormId === c.id ? "Close" : "Add transaction"}
                </Button>
              </div>
              {openFormId === c.id && (
                <CardTxnForm onSubmit={(tx) => addCardTransaction(c.id, tx)} onClose={() => setOpenFormId(null)} />
              )}
              <div className="mt-3">
                {txns.length === 0 ? (
                  <div className="text-xs text-[#98A2B3] py-4 text-center">No expenses logged.</div>
                ) : (
                  txns.slice(0, 6).map((tx) => {
                    const cat = CATEGORY_META[tx.category] || CATEGORY_META.Other;
                    const CatIcon = cat.icon;
                    return (
                      <div key={tx.id} className="flex items-center justify-between py-2 border-b border-[#F2F4F7] last:border-0 text-xs">
                        <div className="flex items-center gap-2">
                          <CatIcon size={13} style={{ color: cat.color }} />
                          <span className="text-[#475467]">{tx.date}{tx.note ? ` · ${tx.note}` : ""}</span>
                        </div>
                        <div className="flex items-center gap-2">
                          <span className="font-semibold text-[#F04438]">-{fmtMoney(Math.abs(tx.amount))}</span>
                          <button onClick={() => deleteCardTransaction(c.id, tx.id)} className="text-[#98A2B3] hover:text-[#F04438]">
                            <X size={11} />
                          </button>
                        </div>
                      </div>
                    );
                  })
                )}
              </div>
            </Panel>
          );
        })}
      </div>
    </div>
  );
}

/* ============================== Expenses ============================== */

function Expenses({ cards }) {
  const allExpenses = useMemo(() => {
    const rows = [];
    cards.forEach((c) => {
      (c.transactions || []).forEach((tx) => rows.push({ ...tx, cardLabel: c.label }));
    });
    return rows.sort((a, b) => new Date(b.date) - new Date(a.date));
  }, [cards]);

  const byCategory = useMemo(() => {
    const map = {};
    allExpenses.forEach((tx) => {
      const cat = tx.category || "Other";
      map[cat] = (map[cat] || 0) + Math.abs(tx.amount);
    });
    return map;
  }, [allExpenses]);

  const total = Object.values(byCategory).reduce((s, v) => s + v, 0);

  return (
    <div className="max-w-5xl">
      <h1 className="text-2xl font-bold text-[#101828] mb-6">Expenses</h1>

      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-6">
        <Panel className="lg:col-span-2">
          <h2 className="text-base font-semibold text-[#101828] mb-4">By category</h2>
          {total === 0 ? (
            <div className="text-sm text-[#98A2B3] py-6 text-center">No card expenses yet.</div>
          ) : (
            <div className="flex flex-col gap-3">
              {Object.entries(byCategory).map(([cat, amt]) => {
                const meta = CATEGORY_META[cat] || CATEGORY_META.Other;
                const Icon = meta.icon;
                const pct = (amt / total) * 100;
                return (
                  <div key={cat}>
                    <div className="flex items-center justify-between text-sm mb-1">
                      <span className="flex items-center gap-2 text-[#101828] font-medium">
                        <Icon size={14} style={{ color: meta.color }} /> {cat}
                      </span>
                      <span className="text-[#667085]">{fmtMoney(amt)} · {pct.toFixed(0)}%</span>
                    </div>
                    <div className="h-2 rounded-full bg-[#F2F4F7]">
                      <div className="h-2 rounded-full" style={{ width: `${pct}%`, background: meta.color }} />
                    </div>
                  </div>
                );
              })}
            </div>
          )}
        </Panel>
        <Panel>
          <h2 className="text-base font-semibold text-[#101828] mb-1">Total spent</h2>
          <div className="text-3xl font-bold text-[#101828] mt-2">{fmtMoney(total)}</div>
          <div className="text-xs text-[#98A2B3] mt-1">across {cards.length} cards · {allExpenses.length} transactions</div>
        </Panel>
      </div>

      <Panel>
        <h2 className="text-base font-semibold text-[#101828] mb-4">All expenses</h2>
        {allExpenses.length === 0 ? (
          <div className="text-sm text-[#98A2B3] py-8 text-center">No expenses logged yet.</div>
        ) : (
          <div className="overflow-x-auto">
            <table className="w-full text-sm">
              <thead>
                <tr className="text-left text-[11px] uppercase tracking-wide text-[#98A2B3] border-b border-[#EAECF0]">
                  <th className="pb-2 font-medium">Card</th>
                  <th className="pb-2 font-medium">Category</th>
                  <th className="pb-2 font-medium">Date</th>
                  <th className="pb-2 font-medium">Note</th>
                  <th className="pb-2 font-medium text-right">Amount</th>
                </tr>
              </thead>
              <tbody>
                {allExpenses.map((tx) => {
                  const meta = CATEGORY_META[tx.category] || CATEGORY_META.Other;
                  return (
                    <tr key={tx.id} className="border-b border-[#F2F4F7] last:border-0">
                      <td className="py-2.5 text-[#101828] font-medium">{tx.cardLabel}</td>
                      <td className="py-2.5">
                        <span
                          className="inline-flex items-center gap-1 rounded-full px-2 py-0.5 text-xs font-medium"
                          style={{ background: meta.color + "1A", color: meta.color }}
                        >
                          {tx.category}
                        </span>
                      </td>
                      <td className="py-2.5 text-[#667085]">{tx.date}</td>
                      <td className="py-2.5 text-[#667085]">{tx.note || "—"}</td>
                      <td className="py-2.5 text-right font-semibold text-[#F04438]">-{fmtMoney(Math.abs(tx.amount))}</td>
                    </tr>
                  );
                })}
              </tbody>
            </table>
          </div>
        )}
      </Panel>
    </div>
  );
}

/* ============================== Main App ============================== */

export default function MKBankApp() {
  const [page, setPage] = useState("dashboard");
  const [accounts, setAccounts] = useState(seedAccounts());
  const [cards, setCards] = useState(seedCards());
  const [loaded, setLoaded] = useState(false);

  useEffect(() => {
    (async () => {
      const a = await loadKey("mkbank-accounts", null);
      const c = await loadKey("mkbank-cards", null);
      if (a) setAccounts(a);
      if (c) setCards(c);
      setLoaded(true);
    })();
  }, []);

  useEffect(() => {
    if (loaded) saveKey("mkbank-accounts", accounts);
  }, [accounts, loaded]);

  useEffect(() => {
    if (loaded) saveKey("mkbank-cards", cards);
  }, [cards, loaded]);

  const addTransaction = useCallback((accId, tx) => {
    setAccounts((prev) => prev.map((a) => (a.id === accId ? { ...a, transactions: [...(a.transactions || []), tx] } : a)));
  }, []);
  const deleteTransaction = useCallback((accId, txId) => {
    setAccounts((prev) =>
      prev.map((a) => (a.id === accId ? { ...a, transactions: (a.transactions || []).filter((t) => t.id !== txId) } : a))
    );
  }, []);

  const changeCardNumber = useCallback((cardId, number) => {
    setCards((prev) => prev.map((c) => (c.id === cardId ? { ...c, number } : c)));
  }, []);
  const addCardTransaction = useCallback((cardId, tx) => {
    setCards((prev) => prev.map((c) => (c.id === cardId ? { ...c, transactions: [...(c.transactions || []), tx] } : c)));
  }, []);
  const deleteCardTransaction = useCallback((cardId, txId) => {
    setCards((prev) =>
      prev.map((c) => (c.id === cardId ? { ...c, transactions: (c.transactions || []).filter((t) => t.id !== txId) } : c))
    );
  }, []);

  return (
    <div className="min-h-screen w-full bg-[#F7F8FA] flex" style={{ fontFamily: "Inter, system-ui, sans-serif" }}>
      <Sidebar page={page} setPage={setPage} />
      <main className="flex-1 p-8 overflow-y-auto">
        {page === "dashboard" && <Dashboard accounts={accounts} />}
        {page === "finance" && <Finance accounts={accounts} />}
        {page === "accounts" && (
          <Accounts accounts={accounts} addTransaction={addTransaction} deleteTransaction={deleteTransaction} />
        )}
        {page === "cards" && (
          <Cards
            cards={cards}
            changeCardNumber={changeCardNumber}
            addCardTransaction={addCardTransaction}
            deleteCardTransaction={deleteCardTransaction}
          />
        )}
        {page === "expenses" && <Expenses cards={cards} />}
      </main>
    </div>
  );
}
