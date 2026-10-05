<script lang="ts">
	type Jadwal = {
		title: string;
		mode: 'Online' | 'Offline';
		status: 'Open' | 'Closed' | 'Full';
		tanggal: string;
		iso: string;
		jam: string;
		peserta: number;
		kuota: number;
		lokasi: string;
		hargaInt: string;
		hargaExt: string;
	};

	const data: Jadwal[] = [
		{ title: 'TEST TOAFL (B. ARAB) 09 JUNI 2026', mode: 'Online', status: 'Open', tanggal: '09 Jun 2026', iso: '2026-06-09', jam: '08:00:00 - 19:00:00', peserta: 2, kuota: 50, lokasi: 'Lab. P2BA Gedung Rektorat Lantai 2', hargaInt: 'Rp 75.000', hargaExt: 'Rp 75.000' },
		{ title: 'TEST TOAFL (B. ARAB) 30 MEI 2026', mode: 'Online', status: 'Open', tanggal: '30 May 2026', iso: '2026-05-30', jam: '08:00:00 - 19:00:00', peserta: 1, kuota: 50, lokasi: 'Lab. P2BA Gedung Rektorat Lantai 2', hargaInt: 'Rp 75.000', hargaExt: 'Rp 75.000' },
		{ title: 'TETS TOEFL (B. INGGRIS) 23 MEI 2026', mode: 'Online', status: 'Open', tanggal: '23 May 2026', iso: '2026-05-23', jam: '08:00:00 - 19:00:00', peserta: 1, kuota: 50, lokasi: 'Lab. P2BA Gedung Rektorat Lantai 2', hargaInt: 'Rp 75.000', hargaExt: 'Rp 75.000' },
		{ title: 'TEST TOAFL (B. ARAB)', mode: 'Online', status: 'Open', tanggal: '23 May 2026', iso: '2026-05-23', jam: '08:00:00 - 19:00:00', peserta: 12, kuota: 50, lokasi: 'Lab. P2BA Gedung Rektorat Lantai 2', hargaInt: 'Rp 75.000', hargaExt: 'Rp 100.000' },
		{ title: 'TEST TOAFL (B. ARAB) 21 MEI 2026', mode: 'Online', status: 'Open', tanggal: '21 May 2026', iso: '2026-05-21', jam: '08:00:00 - 19:00:00', peserta: 8, kuota: 50, lokasi: 'Lab. P2BA Gedung Rektorat Lantai 2', hargaInt: 'Rp 75.000', hargaExt: 'Rp 75.000' },
		{ title: 'TEST TOEFL (B. INGGRIS) 21 MEI 2026', mode: 'Online', status: 'Open', tanggal: '21 May 2026', iso: '2026-05-21', jam: '08:00:00 - 19:00:00', peserta: 25, kuota: 50, lokasi: 'Lab. P2BA Gedung Rektorat Lantai 2', hargaInt: 'Rp 75.000', hargaExt: 'Rp 100.000' },
		{ title: 'TEST TOEFL (B. INGGRIS) 14 MEI 2026', mode: 'Offline', status: 'Closed', tanggal: '14 May 2026', iso: '2026-05-14', jam: '08:00:00 - 12:00:00', peserta: 50, kuota: 50, lokasi: 'Lab. P2BA Gedung Rektorat Lantai 2', hargaInt: 'Rp 75.000', hargaExt: 'Rp 100.000' },
		{ title: 'TEST TOAFL (B. ARAB) 07 MEI 2026', mode: 'Offline', status: 'Closed', tanggal: '07 May 2026', iso: '2026-05-07', jam: '08:00:00 - 12:00:00', peserta: 47, kuota: 50, lokasi: 'Lab. P2BA Gedung Rektorat Lantai 2', hargaInt: 'Rp 75.000', hargaExt: 'Rp 75.000' },
		{ title: 'TEST TOEFL (B. INGGRIS) 30 APR 2026', mode: 'Online', status: 'Full', tanggal: '30 Apr 2026', iso: '2026-04-30', jam: '13:00:00 - 17:00:00', peserta: 50, kuota: 50, lokasi: 'Lab. P2BA Gedung Rektorat Lantai 2', hargaInt: 'Rp 75.000', hargaExt: 'Rp 100.000' }
	];

	let fStatus = $state('Semua');
	let fMode = $state('Semua');
	let fDari = $state('');
	let fSampai = $state('');

	let sStatus = $state('Semua');
	let sMode = $state('Semua');
	let sDari = $state('');
	let sSampai = $state('');

	function apply() {
		sStatus = fStatus;
		sMode = fMode;
		sDari = fDari;
		sSampai = fSampai;
	}
	function reset() {
		fStatus = fMode = 'Semua';
		fDari = fSampai = '';
		apply();
	}

	let filtered = $derived(
		data.filter((j) => {
			if (sStatus !== 'Semua' && j.status !== sStatus) return false;
			if (sMode !== 'Semua' && j.mode !== sMode) return false;
			if (sDari && j.iso < sDari) return false;
			if (sSampai && j.iso > sSampai) return false;
			return true;
		})
	);

	function pct(j: Jadwal) {
		return Math.min(100, Math.round((j.peserta / j.kuota) * 100));
	}
</script>

<svelte:head>
	<title>Jadwal Ujian — P2BA UNIA</title>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap"
		rel="stylesheet"
	/>
	<link
		rel="stylesheet"
		href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
	/>
</svelte:head>

<div class="page">
	<aside class="sidebar">
		<div class="sidebar-header">
			<i class="fa-solid fa-mosque"></i>
			<h2>P2BA UNIA</h2>
			<p>Pusat Bahasa & Tes Akademik</p>
		</div>
		<nav class="sidebar-menu">
			<a href="/admin/dashboard" class="nav-item">
				<i class="fa-solid fa-chart-pie"></i><span>Dashboard</span>
			</a>
			<p class="menu-label">OPERASIONAL</p>
			<a href="/admin/peserta-tes" class="nav-item">
				<i class="fa-solid fa-user-group"></i><span>Peserta Tes</span>
			</a>
			<a href="/admin/jadwal-ujian" class="nav-item active">
				<i class="fa-regular fa-calendar-days"></i><span>Jadwal Ujian</span>
			</a>
			<a href="/admin/kategori-soal" class="nav-item">
				<i class="fa-solid fa-table-cells-large"></i><span>Kategori Soal</span>
			</a>
			<a href="/admin/dashboard" class="nav-item">
				<i class="fa-solid fa-book-open"></i><span>Bank Soal</span>
			</a>
			<a href="/admin/dashboard" class="nav-item">
				<i class="fa-solid fa-file-lines"></i><span>Passage/Teks</span>
			</a>
			<a href="/admin/dashboard" class="nav-item">
				<i class="fa-solid fa-square-poll-vertical"></i><span>Hasil & Skor</span>
			</a>
			<a href="/admin/dashboard" class="nav-item">
				<i class="fa-solid fa-certificate"></i><span>Sertifikat</span>
			</a>
			<p class="menu-label">KEUANGAN</p>
			<a href="/admin/peserta-tes" class="nav-item">
				<i class="fa-solid fa-users"></i><span>Peserta Tes</span>
			</a>
			<a href="/admin/dashboard" class="nav-item">
				<i class="fa-solid fa-wallet"></i><span>Verifikasi Bayar</span>
			</a>
			<a href="/admin/dashboard" class="nav-item">
				<i class="fa-solid fa-file-invoice"></i><span>Laporan</span>
			</a>
			<p class="menu-label">SISTEM</p>
			<a href="/admin/dashboard" class="nav-item">
				<i class="fa-solid fa-user-gear"></i><span>Admin Users</span>
			</a>
			<a href="/admin/dashboard" class="nav-item">
				<i class="fa-solid fa-gear"></i><span>Pengaturan</span>
			</a>
		</nav>
		<div class="sidebar-footer">
			<div class="sidebar-user">
				<div class="sidebar-avatar">S</div>
				<div class="sidebar-user-info"><h4>Super Admin</h4><p>Super admin</p></div>
			</div>
			<a href="/login" class="logout-btn">
				<i class="fa-solid fa-arrow-right-from-bracket"></i><span>Logout</span>
			</a>
		</div>
	</aside>

	<main class="main-content">
		<header class="top-header">
			<div class="header-title">
				<h1>Manajemen Jadwal Ujian</h1>
				<p>Overview data dan statistik</p>
			</div>
			<div class="header-right">
				<button class="notification-btn" aria-label="Notifications">
					<i class="fa-regular fa-bell"></i><span class="notification-badge">3</span>
				</button>
				<div class="vdiv"></div>
				<a href="/login" class="logout-link"
					><i class="fa-solid fa-arrow-right-from-bracket"></i> Keluar</a
				>
			</div>
		</header>

		<div class="container">
			<div class="toolbar">
				<p>Kelola jadwal ujian TOEFL</p>
				<a href="/admin/jadwal-ujian/tambah" class="btn-primary"><i class="fa-solid fa-plus"></i> Buat Jadwal</a>
			</div>

			<section class="card filter-card">
				<div class="filter-grid">
					<label><span>Status</span>
						<select bind:value={fStatus}>
							<option>Semua</option><option>Open</option><option>Closed</option><option>Full</option>
						</select>
					</label>
					<label><span>Mode</span>
						<select bind:value={fMode}>
							<option>Semua</option><option>Online</option><option>Offline</option>
						</select>
					</label>
					<label><span>Dari Tanggal</span><input type="date" bind:value={fDari} /></label>
					<label><span>Sampai Tanggal</span><input type="date" bind:value={fSampai} /></label>
					<div class="filter-btns">
						<button class="btn-primary" onclick={apply}><i class="fa-solid fa-filter"></i> Filter</button>
						<button class="btn-gray" onclick={reset}>Reset</button>
					</div>
				</div>
			</section>

			<section class="grid">
				{#each filtered as j}
					<article class="card jadwal">
						<div class="jhead">
							<span class="mode">{j.mode}</span>
							<span class="st {j.status.toLowerCase()}">{j.status}</span>
							<h3>{j.title}</h3>
						</div>
						<div class="jbody">
							<p><i class="fa-regular fa-calendar"></i> {j.tanggal}</p>
							<p><i class="fa-regular fa-clock"></i> {j.jam}</p>
							<p><i class="fa-solid fa-users"></i> {j.peserta}/{j.kuota} peserta</p>
							<p><i class="fa-regular fa-location-dot"></i> {j.lokasi}</p>
							<div class="kuota">
								<div class="kuota-top"><span>Kuota</span><span>{pct(j)}%</span></div>
								<div class="bar"><div class="fill" style="width:{pct(j)}%"></div></div>
							</div>
							<div class="price">
								<div class="pbox int"><span>Internal</span><strong>{j.hargaInt}</strong></div>
								<div class="pbox ext"><span>Eksternal</span><strong>{j.hargaExt}</strong></div>
							</div>
						</div>
						<div class="jfoot">
							<button class="detail"><i class="fa-regular fa-eye"></i> Detail</button>
							<div class="icons">
								<button aria-label="Edit"><i class="fa-regular fa-pen-to-square"></i></button>
								<button aria-label="Kunci"><i class="fa-solid fa-lock"></i></button>
							</div>
						</div>
					</article>
				{/each}
				{#if filtered.length === 0}
					<p class="empty">Tidak ada jadwal yang cocok dengan filter.</p>
				{/if}
			</section>
		</div>
	</main>
</div>

<style>
	:root {
		--bg-main: #f7f8f7; --bg-card: #fff; --primary: #3b82f6; --primary-dark: #2563eb;
		--primary-light: #eff6ff; --primary-gradient: linear-gradient(135deg, #2563eb, #3b82f6);
		--text-dark: #1e293b; --text-muted: #64748b; --border-color: #f1f5f9;
		--shadow-sm: 0 4px 6px -1px rgba(0,0,0,0.03), 0 2px 4px -1px rgba(0,0,0,0.02);
		--radius: 14px;
	}
	:global(*) { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Plus Jakarta Sans', sans-serif; }
	.page { background: var(--bg-main); color: var(--text-dark); display: flex; min-height: 100vh; }
	a { text-decoration: none; color: inherit; }
	.sidebar { width: 250px; background: var(--bg-card); display: flex; flex-direction: column; border-right: 1px solid #e2e8f0; flex-shrink: 0; position: sticky; top: 0; height: 100vh; height: 100dvh; }
	.sidebar-header { background: var(--primary-gradient); color: #fff; padding: 24px 20px; text-align: center; border-bottom-right-radius: 20px; flex-shrink: 0; }
	.sidebar-header i { font-size: 32px; margin-bottom: 8px; }
	.sidebar-header h2 { font-size: 18px; font-weight: 700; letter-spacing: 1px; }
	.sidebar-header p { font-size: 11px; opacity: 0.8; margin-top: 4px; }
	.sidebar-menu { padding: 20px 12px; flex: 1; display: flex; flex-direction: column; gap: 4px; overflow-y: auto; min-height: 0; scrollbar-width: thin; scrollbar-color: #cbd5e1 transparent; }
	.sidebar-menu::-webkit-scrollbar { width: 6px; }
	.sidebar-menu::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 99px; }
	.menu-label { font-size: 10px; font-weight: 700; letter-spacing: 1px; color: var(--text-muted); opacity: 0.8; margin: 12px 16px 4px; }
	.nav-item { display: flex; align-items: center; gap: 12px; padding: 12px 16px; border-radius: 10px; color: var(--text-muted); font-size: 14px; font-weight: 500; transition: all 0.2s ease; }
	.nav-item:hover { background: #f8fafc; color: var(--primary); }
	.nav-item.active { background: var(--primary); color: #fff; box-shadow: 0 4px 12px rgba(59,130,246,0.3); }
	.sidebar-footer { padding: 16px 12px; border-top: 1px solid var(--border-color); display: flex; flex-direction: column; gap: 8px; flex-shrink: 0; background: var(--bg-card); }
	.sidebar-user { display: flex; align-items: center; gap: 10px; padding: 10px 12px; background: #f8fafc; border-radius: 10px; }
	.sidebar-avatar { width: 32px; height: 32px; border-radius: 50%; background: var(--primary); color: #fff; display: flex; align-items: center; justify-content: center; font-weight: 700; }
	.sidebar-user-info h4 { font-size: 13px; }
	.sidebar-user-info p { font-size: 11px; color: var(--text-muted); }
	.logout-btn { display: flex; align-items: center; gap: 12px; padding: 12px 16px; color: #ef4444; font-weight: 600; font-size: 14px; border-radius: 10px; }
	.logout-btn:hover { background: #fef2f2; }

	.main-content { flex: 1; display: flex; flex-direction: column; min-width: 0; }
	.top-header { background: #fff; padding: 14px 28px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #eef2ef; position: sticky; top: 0; z-index: 5; }
	.header-title h1 { font-size: 20px; font-weight: 800; }
	.header-title p { font-size: 12px; color: var(--text-muted); margin-top: 2px; }
	.header-right { display: flex; align-items: center; gap: 16px; }
	.notification-btn { position: relative; background: none; border: none; font-size: 18px; color: #94a3b8; cursor: pointer; }
	.notification-badge { position: absolute; top: -6px; right: -6px; background: #ef4444; color: #fff; font-size: 10px; width: 16px; height: 16px; border-radius: 50%; display: flex; align-items: center; justify-content: center; border: 2px solid #fff; }
	.vdiv { width: 1px; height: 24px; background: #e2e8f0; }
	.logout-link { font-size: 13px; font-weight: 600; color: #64748b; display: flex; gap: 8px; align-items: center; }

	.container { padding: 20px 24px 28px; display: flex; flex-direction: column; gap: 14px; }
	.toolbar { display: flex; justify-content: space-between; align-items: center; }
	.toolbar p { font-size: 13.5px; color: #475569; }
	.card { background: var(--bg-card); border-radius: var(--radius); box-shadow: var(--shadow-sm); }
	.filter-card { padding: 18px 20px; }
	.filter-grid { display: grid; grid-template-columns: 1fr 1fr 1fr 1fr auto; gap: 12px; align-items: end; }
	.filter-grid label span { font-size: 12px; font-weight: 600; display: block; margin-bottom: 6px; }
	.filter-grid input, .filter-grid select { width: 100%; padding: 10px 14px; border-radius: 10px; border: 1px solid #e2e8f0; font-size: 13px; outline: none; background: #fff; color: var(--text-dark); }
	.filter-grid input:focus, .filter-grid select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px var(--primary-light); }
	.filter-btns { display: flex; gap: 8px; }
	.btn-primary { background: var(--primary); color: #fff; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 700; cursor: pointer; display: inline-flex; gap: 8px; align-items: center; box-shadow: 0 4px 12px rgba(59,130,246,0.3); }
	.btn-primary:hover { background: var(--primary-dark); }
	.btn-gray { background: #eef2f7; color: #334155; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 600; cursor: pointer; }

	.grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
	.jadwal { overflow: hidden; padding: 0; display: flex; flex-direction: column; }
	.jhead { background: var(--primary-gradient); color: #fff; padding: 14px 16px 16px; }
	.jhead .mode { font-size: 11.5px; opacity: 0.9; }
	.jhead .st { float: right; font-size: 10.5px; font-weight: 700; background: rgba(255,255,255,0.25); padding: 3px 10px; border-radius: 99px; }
	.jhead .st.closed { background: rgba(0,0,0,0.25); }
	.jhead .st.full { background: #f59e0b; }
	.jhead h3 { font-size: 14px; font-weight: 800; margin-top: 8px; line-height: 1.4; }
	.jbody { padding: 14px 16px; display: flex; flex-direction: column; gap: 8px; font-size: 12.5px; color: #475569; }
	.jbody i { width: 16px; color: #94a3b8; margin-right: 6px; }
	.kuota-top { display: flex; justify-content: space-between; font-size: 11px; color: var(--text-muted); margin: 6px 0 4px; }
	.bar { height: 6px; background: #eef2f7; border-radius: 99px; overflow: hidden; }
	.fill { height: 100%; background: var(--primary); border-radius: 99px; }
	.price { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 8px; }
	.pbox { border-radius: 10px; padding: 8px; text-align: center; }
	.pbox span { font-size: 10.5px; display: block; }
	.pbox strong { font-size: 13px; }
	.pbox.int { background: var(--primary-light); }
	.pbox.int span { color: var(--primary-dark); }
	.pbox.int strong { color: #1d4ed8; }
	.pbox.ext { background: #faf5ff; }
	.pbox.ext span { color: #8b5cf6; }
	.pbox.ext strong { color: #7c3aed; }
	.jfoot { display: flex; justify-content: space-between; align-items: center; border-top: 1px solid var(--border-color); padding: 10px 16px; margin-top: auto; }
	.detail { background: none; border: none; color: var(--primary-dark); font-size: 12.5px; font-weight: 600; cursor: pointer; display: flex; gap: 6px; align-items: center; }
	.icons { display: flex; gap: 4px; }
	.icons button { background: none; border: none; color: #64748b; cursor: pointer; padding: 6px; border-radius: 8px; }
	.icons button:hover { background: var(--primary-light); color: var(--primary); }
	.empty { color: var(--text-muted); font-size: 13px; }

	@media (max-width: 1200px) { .grid { grid-template-columns: repeat(2, 1fr); } .filter-grid { grid-template-columns: 1fr 1fr; } }
	@media (max-width: 768px) {
		.page { flex-direction: column; }
		.sidebar { width: 100%; position: static; height: auto; }
		.grid { grid-template-columns: 1fr; }
		.filter-grid { grid-template-columns: 1fr; }
	}
</style>
