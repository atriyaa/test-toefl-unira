<script lang="ts">
	type Peserta = {
		nama: string;
		email: string;
		tipe: 'Internal' | 'Eksternal';
		nim: string;
		status: 'Terverifikasi' | 'Menunggu' | 'Ditolak';
		tanggal: string;
	};

	const base: Peserta[] = [
		{ nama: 'ADY AVNAN', email: 'ady@gmail.com', tipe: 'Eksternal', nim: 'Mahasiswa MADIN', status: 'Terverifikasi', tanggal: '09 Jun 2026' },
		{ nama: 'ADY AVNAN', email: 'avnan@gmail.com', tipe: 'Eksternal', nim: 'Mahasiswa MADIN', status: 'Terverifikasi', tanggal: '08 Jun 2026' },
		{ nama: 'ZUBAIDI', email: 'nanikidy@gmil.com', tipe: 'Internal', nim: '20229603064', status: 'Terverifikasi', tanggal: '23 May 2026' },
		{ nama: 'JUNAIDI', email: 'junaidipro29@gmail.com', tipe: 'Internal', nim: '20229603078', status: 'Terverifikasi', tanggal: '23 May 2026' },
		{ nama: 'AHMAD BAHRUL MUGHDIQI', email: 'ahmad.diki121@gmail.com', tipe: 'Internal', nim: '20229603076', status: 'Terverifikasi', tanggal: '21 May 2026' },
		{ nama: 'HABIBULLAH', email: 'habibullahgokil20@gmail.com', tipe: 'Internal', nim: '20229603003', status: 'Terverifikasi', tanggal: '21 May 2026' },
		{ nama: 'NUR JANNAH', email: 'jannah@gmail.com', tipe: 'Eksternal', nim: 'USIM', status: 'Terverifikasi', tanggal: '20 May 2026' },
		{ nama: 'ADELIA HAMZAH', email: 'adelia.hamzah@gmail.com', tipe: 'Internal', nim: '20229604005', status: 'Terverifikasi', tanggal: '20 May 2026' }
	];

	const extraNames: Array<[string, string, 'Internal' | 'Eksternal', string, string]> = [
		['SITI AMINAH', 'siti.aminah@gmail.com', 'Internal', '20229604011', '19 May 2026'],
		['MUHAMMAD RIZKI', 'rizki.m@gmail.com', 'Eksternal', 'STAIN Bengkalis', '19 May 2026'],
		['FATIMAH ZAHRA', 'fatimah.z@gmail.com', 'Internal', '20229604022', '18 May 2026'],
		['ABDUL HAKIM', 'abdul.hakim@gmail.com', 'Internal', '20229603040', '18 May 2026'],
		['RINA WULANDARI', 'rina.w@gmail.com', 'Eksternal', 'UNRI', '17 May 2026'],
		['DEDI SUPRIADI', 'dedi.s@gmail.com', 'Internal', '20229603055', '17 May 2026'],
		['MAYA PUTRI', 'maya.putri@gmail.com', 'Eksternal', 'UIN Suska', '16 May 2026'],
		['AGUS SETIAWAN', 'agus.s@gmail.com', 'Internal', '20229604031', '16 May 2026'],
		['LINA MARLINA', 'lina.m@gmail.com', 'Internal', '20229604038', '15 May 2026'],
		['BUDI HARTONO', 'budi.h@gmail.com', 'Eksternal', 'Polbeng', '15 May 2026'],
		['SARI DEWI', 'sari.dewi@gmail.com', 'Internal', '20229604044', '14 May 2026'],
		['ANDI PRATAMA', 'andi.p@gmail.com', 'Internal', '20229603071', '14 May 2026'],
		['NINA KURNIA', 'nina.k@gmail.com', 'Eksternal', 'UMRI', '13 May 2026'],
		['HENDRA GUNAWAN', 'hendra.g@gmail.com', 'Internal', '20229603089', '13 May 2026'],
		['YULI ASTUTI', 'yuli.a@gmail.com', 'Internal', '20229604052', '12 May 2026'],
		['RUDI HARTONO', 'rudi.h@gmail.com', 'Eksternal', 'STIE Syariah', '12 May 2026'],
		['DEWI LESTARI', 'dewi.l@gmail.com', 'Internal', '20229604060', '11 May 2026'],
		['FARHAN AZIZ', 'farhan.a@gmail.com', 'Internal', '20229603095', '11 May 2026'],
		['INTAN PERMATA', 'intan.p@gmail.com', 'Eksternal', 'UNILAK', '10 May 2026'],
		['GALIH PRAKASA', 'galih.p@gmail.com', 'Internal', '20229604101', '10 May 2026'],
		['WAHYU HIDAYAT', 'wahyu.h@gmail.com', 'Internal', '20229603110', '09 May 2026'],
		['PUTRI AYU', 'putri.ayu@gmail.com', 'Eksternal', 'UIR', '09 May 2026'],
		['DANI RAMADHAN', 'dani.r@gmail.com', 'Internal', '20229603118', '08 May 2026'],
		['EKA SAPUTRA', 'eka.s@gmail.com', 'Internal', '20229603122', '08 May 2026'],
		['TARI WULAN', 'tari.w@gmail.com', 'Eksternal', 'STAI', '07 May 2026'],
		['BAGUS NUGROHO', 'bagus.n@gmail.com', 'Internal', '20229603130', '07 May 2026'],
		['CITRA LESTARI', 'citra.l@gmail.com', 'Internal', '20229604115', '06 May 2026'],
		['YOGA SAPUTRA', 'yoga.s@gmail.com', 'Eksternal', 'PCR', '06 May 2026'],
		[' Ratna Sari', 'ratna.s@gmail.com', 'Internal', '20229604120', '05 May 2026'],
		['ILHAM MAULANA', 'ilham.m@gmail.com', 'Internal', '20229603141', '05 May 2026']
	];

	const allData: Peserta[] = [
		...base,
		...extraNames.map(([nama, email, tipe, nim, tanggal], i) => ({
			nama: nama.trim(),
			email,
			tipe,
			nim,
			status: (i % 9 === 4 ? 'Menunggu' : i % 11 === 7 ? 'Ditolak' : 'Terverifikasi') as Peserta['status'],
			tanggal
		})),
		...Array.from({ length: 24 }, (_, i) => ({
			nama: `PESERTA CADANGAN ${i + 1}`,
			email: `cadangan${i + 1}@mail.com`,
			tipe: (i % 2 === 0 ? 'Internal' : 'Eksternal') as Peserta['tipe'],
			nim: i % 2 === 0 ? `20229605${String(100 + i)}` : 'Umum / Eksternal',
			status: (i % 8 === 3 ? 'Menunggu' : 'Terverifikasi') as Peserta['status'],
			tanggal: `${String(28 - (i % 28)).padStart(2, '0')} Apr 2026`
		}))
	];

	let q = $state('');
	let tipe = $state('Semua');
	let status = $state('Semua');
	let page = $state(1);
	let perPage = $state(25);

	let filtered = $derived(
		allData.filter((p) => {
			const s = q.trim().toLowerCase();
			const matchQ =
				!s ||
				p.nama.toLowerCase().includes(s) ||
				p.email.toLowerCase().includes(s) ||
				p.nim.toLowerCase().includes(s);
			const matchT = tipe === 'Semua' || p.tipe === tipe;
			const matchS = status === 'Semua' || p.status === status;
			return matchQ && matchT && matchS;
		})
	);

	let totalPages = $derived(Math.max(1, Math.ceil(filtered.length / perPage)));
	let safePage = $derived(Math.min(page, totalPages));
	let startIdx = $derived((safePage - 1) * perPage);
	let rows = $derived(filtered.slice(startIdx, startIdx + perPage));
	let from = $derived(filtered.length === 0 ? 0 : startIdx + 1);
	let to = $derived(Math.min(startIdx + perPage, filtered.length));

	let pageNumbers = $derived(() => {
		const total = totalPages;
		const cur = safePage;
		let start = Math.max(1, cur - 2);
		let end = Math.min(total, start + 4);
		if (end - start < 4) start = Math.max(1, end - 4);
		return Array.from({ length: end - start + 1 }, (_, i) => start + i);
	});

	function applyFilter() {
		page = 1;
	}
	function resetFilter() {
		q = '';
		tipe = 'Semua';
		status = 'Semua';
		page = 1;
	}
	function initial(nama: string) {
		return (nama.trim().charAt(0) || '?').toUpperCase();
	}
</script>

<svelte:head>
	<title>Peserta Tes — P2BA UNIA</title>
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
			<a href="/admin/peserta-tes" class="nav-item active">
				<i class="fa-solid fa-user-group"></i><span>Peserta Tes</span>
			</a>
			<a href="/admin/jadwal-ujian" class="nav-item">
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
				<h1>Manajemen Peserta</h1>
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
			<section class="card filter-card">
				<div class="filter-grid">
					<label class="fgrow">
						<span>Cari</span>
						<input
							type="text"
							placeholder="Nama, email, NIM..."
							bind:value={q}
							onkeydown={(e) => e.key === 'Enter' && applyFilter()}
						/>
					</label>
					<label>
						<span>Tipe</span>
						<select bind:value={tipe}>
							<option>Semua</option>
							<option>Internal</option>
							<option>Eksternal</option>
						</select>
					</label>
					<label>
						<span>Status Verifikasi</span>
						<select bind:value={status}>
							<option>Semua</option>
							<option>Terverifikasi</option>
							<option>Menunggu</option>
							<option>Ditolak</option>
						</select>
					</label>
					<div class="filter-btns">
						<button class="btn-green" onclick={applyFilter}
							><i class="fa-solid fa-magnifying-glass"></i> Filter</button
						>
						<button class="btn-gray" onclick={resetFilter}>Reset</button>
					</div>
				</div>
			</section>

			<div class="toolbar">
				<span class="count">{filtered.length} peserta ditemukan</span>
				<a href="/admin/peserta-tes/tambah" class="btn-green solid"><i class="fa-solid fa-plus"></i> Tambah Peserta</a>
			</div>

			<section class="card table-card">
				<div class="table-responsive">
					<table>
						<thead>
							<tr>
								<th>PESERTA</th>
								<th>TIPE</th>
								<th>NIM/INSTITUSI</th>
								<th>STATUS</th>
								<th>TERDAFTAR</th>
								<th class="right">AKSI</th>
							</tr>
						</thead>
						<tbody>
							{#each rows as p}
								<tr>
									<td>
										<div class="peserta">
											<div class="avatar">{initial(p.nama)}</div>
											<div><h4>{p.nama}</h4><p>{p.email}</p></div>
										</div>
									</td>
									<td>
										<span class="pill {p.tipe === 'Internal' ? 'blue' : 'purple'}">{p.tipe}</span>
									</td>
									<td class="nim">{p.nim}</td>
									<td>
										{#if p.status === 'Terverifikasi'}
											<span class="pill green"
												><i class="fa-solid fa-circle-check"></i> Terverifikasi</span
											>
										{:else if p.status === 'Menunggu'}
											<span class="pill yellow"
												><i class="fa-regular fa-clock"></i> Menunggu</span
											>
										{:else}
											<span class="pill red"
												><i class="fa-solid fa-circle-xmark"></i> Ditolak</span
											>
										{/if}
									</td>
									<td class="date">{p.tanggal}</td>
									<td class="right actions">
										<button title="Reset key" aria-label="key"><i class="fa-solid fa-key"></i></button>
										<button title="Detail" aria-label="detail"><i class="fa-regular fa-eye"></i></button>
										<button title="Edit" aria-label="edit"
											><i class="fa-regular fa-pen-to-square"></i></button
										>
									</td>
								</tr>
							{/each}
							{#if rows.length === 0}
								<tr><td colspan="6" class="empty">Tidak ada data yang cocok dengan filter.</td></tr>
							{/if}
						</tbody>
					</table>
				</div>

				<div class="pagination">
					<p class="info">Menampilkan {from}–{to} dari {filtered.length} data</p>
					<div class="pager">
						<label class="perpage">
							<select bind:value={perPage} onchange={() => (page = 1)} aria-label="Data per halaman">
								<option value={25}>25 / page</option>
								<option value={50}>50 / page</option>
							</select>
						</label>
						<button
							class="pg"
							disabled={safePage <= 1}
							onclick={() => (page = Math.max(1, safePage - 1))}
							aria-label="Sebelumnya">‹</button
						>
						{#each pageNumbers() as n}
							<button class="pg {n === safePage ? 'active' : ''}" onclick={() => (page = n)}>{n}</button>
						{/each}
						<button
							class="pg"
							disabled={safePage >= totalPages}
							onclick={() => (page = Math.min(totalPages, safePage + 1))}
							aria-label="Berikutnya">›</button
						>
					</div>
				</div>
			</section>
		</div>
	</main>
</div>

<style>
	:root {
		--bg-main: #f7f8f7;
		--bg-card: #ffffff;
		--primary: #3b82f6;
		--primary-dark: #2563eb;
		--primary-gradient: linear-gradient(135deg, #2563eb, #3b82f6);
		--text-dark: #1e293b;
		--text-muted: #64748b;
		--border-color: #f1f5f9;
		--shadow-sm: 0 4px 6px -1px rgba(0, 0, 0, 0.03), 0 2px 4px -1px rgba(0, 0, 0, 0.02);
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
	.sidebar-menu { padding: 20px 12px; flex: 1; display: flex; flex-direction: column; gap: 4px; overflow-y: auto; overflow-x: hidden; min-height: 0; scrollbar-width: thin; scrollbar-color: #cbd5e1 transparent; }
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
	.card { background: var(--bg-card); border-radius: var(--radius); box-shadow: var(--shadow-sm); padding: 18px 20px; }
	.filter-grid { display: grid; grid-template-columns: 2fr 1fr 1fr auto; gap: 12px; align-items: end; }
	.filter-grid label span { font-size: 12px; font-weight: 600; display: block; margin-bottom: 6px; }
	.filter-grid input, .filter-grid select { width: 100%; padding: 10px 14px; border-radius: 10px; border: 1px solid #e2e8f0; background: #fff; font-size: 13px; outline: none; color: var(--text-dark); }
	.filter-grid input:focus, .filter-grid select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px var(--primary-light); }
	.filter-btns { display: flex; gap: 8px; }
	.btn-green { background: var(--primary); color: #fff; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 700; cursor: pointer; display: flex; gap: 8px; align-items: center; box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3); }
	.btn-green:hover { background: var(--primary-dark); }
	.btn-green.solid { padding: 11px 18px; }
	.btn-gray { background: #eef2f7; color: #334155; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 600; cursor: pointer; }
	.toolbar { display: flex; justify-content: space-between; align-items: center; }
	.count { font-size: 12px; color: var(--text-muted); }

	.table-card { padding: 0; overflow: hidden; }
	.table-responsive { overflow-x: auto; }
	table { width: 100%; border-collapse: collapse; font-size: 13px; }
	th { text-align: left; padding: 12px 16px; color: #64748b; font-size: 11px; letter-spacing: 0.5px; border-bottom: 1px solid var(--border-color); background: #fff; }
	td { padding: 12px 16px; border-bottom: 1px solid var(--border-color); vertical-align: middle; }
	tbody tr:hover { background: #f8fafc; }
	.peserta { display: flex; gap: 12px; align-items: center; }
	.avatar { width: 36px; height: 36px; border-radius: 50%; background: #dbeafe; color: #2563eb; font-weight: 800; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
	.peserta h4 { font-size: 13px; font-weight: 700; }
	.peserta p { font-size: 11.5px; color: var(--text-muted); }
	.nim { color: #475569; }
	.date { color: #64748b; white-space: nowrap; }
	.right { text-align: right; }
	.pill { display: inline-flex; align-items: center; gap: 5px; font-size: 11px; font-weight: 700; padding: 4px 10px; border-radius: 99px; white-space: nowrap; }
	.pill.blue { background: #dbeafe; color: #1d4ed8; }
	.pill.purple { background: #f3e8ff; color: #7c3aed; }
	.pill.green { background: #dcfce7; color: #15803d; }
	.pill.yellow { background: #fef9c3; color: #a16207; }
	.pill.red { background: #fee2e2; color: #b91c1c; }
	.actions { white-space: nowrap; }
	.actions button { background: none; border: none; color: #64748b; cursor: pointer; padding: 6px; border-radius: 8px; font-size: 13px; }
	.actions button:hover { background: #eff6ff; color: var(--primary); }
	.empty { text-align: center; color: var(--text-muted); padding: 28px; }

	.pagination { display: flex; justify-content: space-between; align-items: center; gap: 12px; padding: 14px 16px; flex-wrap: wrap; }
	.info { font-size: 12px; color: var(--text-muted); }
	.pager { display: flex; align-items: center; gap: 6px; }
	.perpage select { padding: 8px 10px; border-radius: 10px; border: 1px solid #e2e8f0; font-size: 12px; font-weight: 600; background: #fff; outline: none; }
	.pg { min-width: 32px; height: 32px; padding: 0 8px; border-radius: 9px; border: 1px solid #e2e8f0; background: #fff; font-size: 12px; font-weight: 700; color: #475569; cursor: pointer; }
	.pg:hover:not(:disabled) { border-color: var(--primary); color: var(--primary-dark); }
	.pg.active { background: var(--primary); border-color: var(--primary); color: #fff; box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3); }
	.pg:disabled { opacity: 0.4; cursor: not-allowed; }

	@media (max-width: 1024px) { .filter-grid { grid-template-columns: 1fr 1fr; } }
	@media (max-width: 768px) {
		.page { flex-direction: column; }
		.sidebar { width: 100%; position: static; height: auto; }
		.filter-grid { grid-template-columns: 1fr; }
		.toolbar { flex-direction: column; gap: 10px; align-items: stretch; }
	}
</style>
