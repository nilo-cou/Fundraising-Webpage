<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Assam Flood Relief | Admin Dashboard</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
    body { font-family: 'Inter', sans-serif; }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 antialiased min-h-screen flex flex-col">
  <header class="bg-white border-b border-slate-200 sticky top-0 z-50">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
      <div class="flex items-center gap-3">
        <div class="bg-blue-600 text-white p-2 rounded-lg">
          <i data-lucide="waves" class="w-5 h-5"></i>
        </div>
        <div>
          <h1 class="font-semibold text-slate-900 leading-none">Assam Flood Relief</h1>
          <span class="text-xs text-slate-500 font-medium">Admin Dashboard</span>
        </div>
      </div>
      <div class="flex items-center gap-3">
        <button id="admin-toggle" class="border border-slate-300 text-slate-700 px-3 py-2 rounded-lg text-sm font-medium hidden">Edit Dashboard</button>
        <input id="quick-donation-amount" type="number" min="1" value="100" class="w-28 border border-slate-300 rounded-lg px-3 py-2 text-sm" />
        <button onclick="openPaymentModal()" class="bg-blue-600 hover:bg-blue-700 text-white text-sm font-medium px-4 py-2 rounded-lg transition-colors flex items-center gap-2">
          <i data-lucide="heart" class="w-4 h-4"></i>
          Payment Option
        </button>
      </div>
    </div>
  </header>

  <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 flex-1 w-full space-y-8">
    <div class="bg-white border border-slate-200 rounded-xl p-6 shadow-sm">
      <h2 class="text-lg font-semibold text-slate-900">Current Relief Status</h2>
      <p class="text-sm text-slate-600 mt-1">All amounts are currently set to zero. Admin can update them from the edit panel.</p>
    </div>

    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
      <div class="bg-white border border-slate-200 rounded-xl p-5 shadow-sm space-y-3">
        <div class="flex justify-between items-center text-slate-500">
          <span class="text-xs font-medium uppercase tracking-wider">Funds Raised</span>
          <i data-lucide="indian-rupee" class="w-4 h-4 text-emerald-600"></i>
        </div>
        <div id="funds-raised" class="text-2xl font-bold text-slate-900">₹0</div>
        <div class="text-xs text-slate-500">Target: <span id="target-amount" class="font-medium text-slate-700">₹0</span> <span id="progress-text">(0%)</span></div>
        <div class="w-full bg-slate-100 rounded-full h-1.5 overflow-hidden">
          <div id="progress-bar" class="bg-emerald-500 h-1.5 rounded-full" style="width: 0%"></div>
        </div>
      </div>

      <div class="bg-white border border-slate-200 rounded-xl p-5 shadow-sm space-y-3">
        <div class="flex justify-between items-center text-slate-500">
          <span class="text-xs font-medium uppercase tracking-wider">Animals Fed</span>
          <i data-lucide="wheat" class="w-4 h-4 text-amber-600"></i>
        </div>
        <div id="animals-fed" class="text-2xl font-bold text-slate-900">0</div>
        <p class="text-xs text-slate-500">Fodder assistance count</p>
      </div>

      <div class="bg-white border border-slate-200 rounded-xl p-5 shadow-sm space-y-3">
        <div class="flex justify-between items-center text-slate-500">
          <span class="text-xs font-medium uppercase tracking-wider">Homes Disinfected</span>
          <i data-lucide="sparkles" class="w-4 h-4 text-blue-600"></i>
        </div>
        <div id="homes-disinfected" class="text-2xl font-bold text-slate-900">0</div>
        <p class="text-xs text-slate-500">Sanitization and cleaning support</p>
      </div>

      <div class="bg-white border border-slate-200 rounded-xl p-5 shadow-sm space-y-3">
        <div class="flex justify-between items-center text-slate-500">
          <span class="text-xs font-medium uppercase tracking-wider">Families Aided</span>
          <i data-lucide="users" class="w-4 h-4 text-indigo-600"></i>
        </div>
        <div id="families-aided" class="text-2xl font-bold text-slate-900">0</div>
        <p class="text-xs text-slate-500">Direct relief support count</p>
      </div>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
      <div class="bg-white border border-slate-200 rounded-xl p-6 shadow-sm lg:col-span-2 space-y-4">
        <h3 class="text-base font-semibold text-slate-900">Allocation Plan</h3>
        <div class="h-64 relative">
          <canvas id="allocationChart"></canvas>
        </div>
      </div>

      <div class="bg-white border border-slate-200 rounded-xl p-6 shadow-sm space-y-4">
        <h3 class="text-base font-semibold text-slate-900">Relief Needs</h3>
        <ul class="space-y-3 text-sm">
          <li class="p-2.5 rounded-lg bg-slate-50">Water jars — ₹0</li>
          <li class="p-2.5 rounded-lg bg-slate-50">Disinfection kits — ₹0</li>
          <li class="p-2.5 rounded-lg bg-slate-50">Clothing kits — ₹0</li>
        </ul>
      </div>
    </div>

    <div class="bg-white border border-slate-200 rounded-xl p-6 shadow-sm">
      <h3 class="text-base font-semibold text-slate-900">Disbursement Log</h3>
      <div class="overflow-x-auto mt-3">
        <table class="w-full text-left text-sm">
          <thead class="bg-slate-50 text-slate-500 uppercase text-[10px] tracking-wider border-b border-slate-200">
            <tr>
              <th class="py-3 px-4">Date</th>
              <th class="py-3 px-4">Category</th>
              <th class="py-3 px-4">Details</th>
              <th class="py-3 px-4 text-right">Amount</th>
            </tr>
          </thead>
          <tbody id="disbursement-body" class="divide-y divide-slate-100 text-slate-700">
            <tr>
              <td colspan="4" class="py-6 text-center text-xs text-slate-400">No disbursements added yet.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </main>

  <div id="admin-panel" class="hidden fixed inset-0 bg-slate-900/50 z-50 flex items-center justify-center p-4">
    <div class="bg-white rounded-xl p-6 w-full max-w-2xl max-h-[90vh] overflow-y-auto space-y-4">
      <div class="flex justify-between items-center">
        <h3 class="text-lg font-semibold text-slate-900">Admin Edit Panel</h3>
        <button id="close-admin" class="text-slate-500">Close</button>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-3 text-sm">
        <label class="space-y-1">
          <span class="text-slate-600">Funds Raised</span>
          <input id="admin-funds" type="number" class="w-full border rounded px-3 py-2" value="0" />
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">Target Amount</span>
          <input id="admin-target" type="number" class="w-full border rounded px-3 py-2" value="0" />
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">Progress %</span>
          <input id="admin-progress" type="number" class="w-full border rounded px-3 py-2" value="0" />
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">Animals Fed</span>
          <input id="admin-animals" type="number" class="w-full border rounded px-3 py-2" value="0" />
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">Homes Disinfected</span>
          <input id="admin-homes" type="number" class="w-full border rounded px-3 py-2" value="0" />
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">Families Aided</span>
          <input id="admin-families" type="number" class="w-full border rounded px-3 py-2" value="0" />
        </label>
        <label class="space-y-1 md:col-span-2">
          <span class="text-slate-600">Disbursement Details</span>
          <textarea id="admin-log" class="w-full border rounded px-3 py-2" rows="3">No disbursements added yet.</textarea>
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">UPI ID</span>
          <input id="admin-upi" type="text" class="w-full border rounded px-3 py-2" value="n.deka97060-1@oksbi" />
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">QR Code URL</span>
          <input id="admin-qr" type="text" class="w-full border rounded px-3 py-2" value="https://api.qrserver.com/v1/create-qr-code/?size=180x180&data=upi://pay?pa=n.deka97060-1%40oksbi%26pn=Assam%20Flood%20Relief" />
        </label>
        <label class="space-y-1 md:col-span-2">
          <span class="text-slate-600">Or Upload QR Image</span>
          <input id="admin-qr-file" type="file" accept="image/*" class="w-full border rounded px-3 py-2" />
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">Account Holder</span>
          <input id="admin-holder" type="text" class="w-full border rounded px-3 py-2" value="YOUR NAME HERE" />
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">Account No</span>
          <input id="admin-account" type="text" class="w-full border rounded px-3 py-2" value="XXXXXXXXXXXX" />
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">IFSC</span>
          <input id="admin-ifsc" type="text" class="w-full border rounded px-3 py-2" value="XXXX0000XXX" />
        </label>
        <label class="space-y-1">
          <span class="text-slate-600">Bank Name</span>
          <input id="admin-bank" type="text" class="w-full border rounded px-3 py-2" value="YOUR BANK NAME" />
        </label>
      </div>

      <div class="flex gap-2">
        <button id="save-admin" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg">Save Changes</button>
        <button id="reset-admin" class="bg-slate-200 px-4 py-2 rounded-lg">Reset to Zero</button>
      </div>
    </div>
  </div>

  <div id="donate-modal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm flex items-center justify-center p-4 z-50 hidden">
    <div class="bg-white rounded-xl shadow-xl border border-slate-200 max-w-md w-full p-6 space-y-5">
      <div class="flex justify-between items-center">
        <h3 class="text-lg font-semibold text-slate-900">Payment Details</h3>
        <button onclick="closePaymentModal()" class="text-slate-400 hover:text-slate-600">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>
      <div class="bg-slate-50 border border-slate-200 rounded-lg p-4 flex flex-col items-center justify-center space-y-3">
        <img id="payment-qr" class="w-44 h-44 object-contain" src="https://api.qrserver.com/v1/create-qr-code/?size=180x180&data=upi://pay?pa=n.deka97060-1%40oksbi%26pn=Assam%20Flood%20Relief" alt="UPI QR" />
        <p class="text-[11px] text-slate-400">Scan with any UPI app</p>
      </div>
      <div class="space-y-2 text-sm">
        <label class="block text-slate-600">UPI ID</label>
        <div class="flex items-center gap-2">
          <input id="payment-upi" type="text" readonly class="w-full border rounded px-3 py-2" value="n.deka97060-1@oksbi" />
          <button onclick="copyUpi()" class="bg-blue-600 text-white px-3 py-2 rounded-lg">Copy</button>
        </div>
        <div class="border-t pt-3 space-y-2">
          <label class="block text-slate-600">Enter donation amount (₹)</label>
          <input id="donation-amount" type="number" min="1" value="100" class="w-full border rounded px-3 py-2" oninput="updatePaymentLink()" />
          <a id="pay-link" href="#" target="_blank" rel="noopener noreferrer" class="w-full inline-flex items-center justify-center bg-emerald-600 hover:bg-emerald-700 text-white px-3 py-2 rounded-lg">Pay with GPay / UPI</a>
          <button onclick="recordDonation()" class="w-full bg-blue-600 hover:bg-blue-700 text-white px-3 py-2 rounded-lg">I Have Paid - Record Donation</button>
          <p class="text-[11px] text-slate-500">Amount is prefilled in the UPI app. After payment, click "I Have Paid" to update this dashboard.</p>
        </div>
      </div>
    </div>
  </div>

  <footer class="bg-white border-t border-slate-200 py-6 text-center text-xs text-slate-500">
    <p>Assam Flood Relief Fund &copy; 2026. Admin-editable dashboard.</p>
  </footer>

  <script>
    lucide.createIcons();

    const STORAGE_KEY = 'relief-dashboard-state';
    const STATE_VERSION = 3;
    const defaultState = {
      fundsRaised: 0,
      targetAmount: 0,
      progressPercent: 0,
      animalsFed: 0,
      homesDisinfected: 0,
      familiesAided: 0,
      disbursementText: 'No disbursements added yet.',
      upiId: 'n.deka97060-1@oksbi',
      qrUrl: 'https://api.qrserver.com/v1/create-qr-code/?size=180x180&data=upi://pay?pa=n.deka97060-1%40oksbi%26pn=Assam%20Flood%20Relief',
      stateVersion: STATE_VERSION,
      accountHolder: 'YOUR NAME HERE',
      accountNo: 'XXXXXXXXXXXX',
      ifsc: 'XXXX0000XXX',
      bankName: 'YOUR BANK NAME'
    };

    function loadState() {
      try {
        const parsed = JSON.parse(localStorage.getItem(STORAGE_KEY));
        const merged = { ...defaultState, ...(parsed || {}) };

        // One-time migration so old cached browsers reset amounts and use latest payment defaults.
        if (!parsed || parsed.stateVersion !== STATE_VERSION) {
          merged.fundsRaised = 0;
          merged.targetAmount = 0;
          merged.progressPercent = 0;
          merged.animalsFed = 0;
          merged.homesDisinfected = 0;
          merged.familiesAided = 0;
          merged.disbursementText = defaultState.disbursementText;
          merged.upiId = defaultState.upiId;
          merged.qrUrl = defaultState.qrUrl;
          merged.stateVersion = STATE_VERSION;
          saveState(merged);
        }

        return merged;
      } catch {
        return { ...defaultState };
      }
    }

    function saveState(state) {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
    }

    function formatMoney(value) {
      return `₹${Number(value || 0).toLocaleString('en-IN')}`;
    }

    function renderDashboard(state) {
      document.getElementById('funds-raised').innerText = formatMoney(state.fundsRaised);
      document.getElementById('target-amount').innerText = formatMoney(state.targetAmount);
      document.getElementById('progress-text').innerText = `(${state.progressPercent || 0}%)`;
      document.getElementById('progress-bar').style.width = `${Math.min(100, Math.max(0, state.progressPercent || 0))}%`;
      document.getElementById('animals-fed').innerText = state.animalsFed;
      document.getElementById('homes-disinfected').innerText = state.homesDisinfected;
      document.getElementById('families-aided').innerText = state.familiesAided;
      document.getElementById('payment-upi').value = state.upiId;
      document.getElementById('payment-qr').src = state.qrUrl;

      const rows = state.disbursementText
        .split('\n')
        .filter(Boolean)
        .map((line) => `<tr><td colspan="4" class="py-3 px-4 text-sm text-slate-600">${line}</td></tr>`)
        .join('');
      document.getElementById('disbursement-body').innerHTML = rows || `<tr><td colspan="4" class="py-6 text-center text-xs text-slate-400">No disbursements added yet.</td></tr>`;
    }

    function openAdminPanel() {
      const state = loadState();
      document.getElementById('admin-funds').value = state.fundsRaised;
      document.getElementById('admin-target').value = state.targetAmount;
      document.getElementById('admin-progress').value = state.progressPercent;
      document.getElementById('admin-animals').value = state.animalsFed;
      document.getElementById('admin-homes').value = state.homesDisinfected;
      document.getElementById('admin-families').value = state.familiesAided;
      document.getElementById('admin-log').value = state.disbursementText;
      document.getElementById('admin-upi').value = state.upiId;
      document.getElementById('admin-qr').value = state.qrUrl;
      document.getElementById('admin-holder').value = state.accountHolder;
      document.getElementById('admin-account').value = state.accountNo;
      document.getElementById('admin-ifsc').value = state.ifsc;
      document.getElementById('admin-bank').value = state.bankName;
      document.getElementById('admin-panel').classList.remove('hidden');
    }

    function closeAdminPanel() {
      document.getElementById('admin-panel').classList.add('hidden');
    }

    function saveAdminChanges() {
      const state = {
        fundsRaised: Number(document.getElementById('admin-funds').value || 0),
        targetAmount: Number(document.getElementById('admin-target').value || 0),
        progressPercent: Number(document.getElementById('admin-progress').value || 0),
        animalsFed: Number(document.getElementById('admin-animals').value || 0),
        homesDisinfected: Number(document.getElementById('admin-homes').value || 0),
        familiesAided: Number(document.getElementById('admin-families').value || 0),
        disbursementText: document.getElementById('admin-log').value || 'No disbursements added yet.',
        upiId: document.getElementById('admin-upi').value || 'n.deka97060-1@oksbi',
        qrUrl: document.getElementById('admin-qr').value || 'https://api.qrserver.com/v1/create-qr-code/?size=180x180&data=upi://pay?pa=n.deka97060-1%40oksbi%26pn=Assam%20Flood%20Relief',
        accountHolder: document.getElementById('admin-holder').value || 'YOUR NAME HERE',
        accountNo: document.getElementById('admin-account').value || 'XXXXXXXXXXXX',
        ifsc: document.getElementById('admin-ifsc').value || 'XXXX0000XXX',
        bankName: document.getElementById('admin-bank').value || 'YOUR BANK NAME'
      };
      saveState(state);
      renderDashboard(state);
      closeAdminPanel();
    }

    function resetAdminToZero() {
      saveState(defaultState);
      renderDashboard(defaultState);
      closeAdminPanel();
    }

    function openPaymentModal() {
      const quickAmount = Number(document.getElementById('quick-donation-amount').value || 0);
      if (quickAmount > 0) {
        document.getElementById('donation-amount').value = quickAmount;
      }
      document.getElementById('donate-modal').classList.remove('hidden');
      updatePaymentLink();
    }

    function closePaymentModal() {
      document.getElementById('donate-modal').classList.add('hidden');
    }

    function copyUpi() {
      const input = document.getElementById('payment-upi');
      input.select();
      navigator.clipboard.writeText(input.value);
    }

    function updatePaymentLink() {
      const state = loadState();
      const amount = Number(document.getElementById('donation-amount').value || 0);
      const safeAmount = amount > 0 ? amount.toFixed(2) : '0.00';
      const upiId = state.upiId || defaultState.upiId;
      const payeeName = 'Assam Flood Relief';
      const note = 'Flood relief donation';

      const upiUrl = `upi://pay?pa=${encodeURIComponent(upiId)}&pn=${encodeURIComponent(payeeName)}&am=${encodeURIComponent(safeAmount)}&cu=INR&tn=${encodeURIComponent(note)}`;
      const payLink = document.getElementById('pay-link');
      payLink.href = upiUrl;

      // Regenerate QR whenever amount changes so QR matches the typed amount.
      const qrUrl = `https://api.qrserver.com/v1/create-qr-code/?size=180x180&data=${encodeURIComponent(upiUrl)}`;
      document.getElementById('payment-qr').src = qrUrl;
    }

    function recordDonation() {
      const amount = Number(document.getElementById('donation-amount').value || 0);
      if (amount <= 0) return;

      const state = loadState();
      const updatedState = {
        ...state,
        fundsRaised: Number(state.fundsRaised || 0) + amount,
        progressPercent: Math.min(100, Math.round(((Number(state.fundsRaised || 0) + amount) / Math.max(Number(state.targetAmount || 1), 1)) * 100)),
        animalsFed: Number(state.animalsFed || 0) + Math.max(1, Math.floor(amount / 500)),
        homesDisinfected: Number(state.homesDisinfected || 0) + Math.max(1, Math.floor(amount / 1000)),
        familiesAided: Number(state.familiesAided || 0) + Math.max(1, Math.floor(amount / 800)),
        disbursementText: `${new Date().toLocaleDateString('en-IN')} — Donation received for ₹${amount.toLocaleString('en-IN')}.\n${state.disbursementText}`
      };

      saveState(updatedState);
      renderDashboard(updatedState);
      closePaymentModal();
      alert(`Donation recorded successfully! Added ₹${amount.toLocaleString('en-IN')} to the dashboard.`);
    }

    const isAdmin = location.search.includes('admin=true');
    const adminToggle = document.getElementById('admin-toggle');
    if (isAdmin) {
      adminToggle.classList.remove('hidden');
    }

    adminToggle.addEventListener('click', openAdminPanel);
    document.getElementById('close-admin').addEventListener('click', closeAdminPanel);
    document.getElementById('save-admin').addEventListener('click', saveAdminChanges);
    document.getElementById('reset-admin').addEventListener('click', resetAdminToZero);
    document.getElementById('admin-qr-file').addEventListener('change', (event) => {
      const file = event.target.files && event.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = () => {
        const dataUrl = String(reader.result || '');
        if (dataUrl) {
          document.getElementById('admin-qr').value = dataUrl;
        }
      };
      reader.readAsDataURL(file);
    });

    const initialState = loadState();
    renderDashboard(initialState);
    updatePaymentLink();

    const chartCanvas = document.getElementById('allocationChart');
    if (chartCanvas && window.Chart) {
      const ctx = chartCanvas.getContext('2d');
      new Chart(ctx, {
        type: 'bar',
        data: {
          labels: ['Water', 'Disinfection', 'Clothes', 'Feed'],
          datasets: [{
            label: 'Allocation',
            data: [0, 0, 0, 0],
            backgroundColor: '#2563eb',
            borderRadius: 6,
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: { legend: { display: false } },
          scales: {
            y: { border: { display: false }, grid: { color: '#f1f5f9' } },
            x: { grid: { display: false } }
          }
        }
      });
    }
  </script>
</body>
</html>
