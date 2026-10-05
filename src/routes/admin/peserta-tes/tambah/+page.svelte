<script lang="ts">
	let tipe: 'internal' | 'eksternal' = $state('internal');
	let nim = $state('');
	let nama = $state('');
	let email = $state('');
	let institusi = $state('');
	let jadwal = $state('');
	let checking = $state(false);
	let found: { nama: string; prodi: string } | null = $state(null);

	const jadwals = [
		'TOEFL ITP — Sabtu, 12 Okt 2026 · 08:00 WIB',
		'TOEFL ITP — Sabtu, 19 Okt 2026 · 08:00 WIB',
		'English Proficiency — Jumat, 25 Okt 2026 · 13:00 WIB'
	];

	function cekData() {
		if (!nim.trim()) return;
		checking = true;
		found = null;
		setTimeout(() => {
			checking = false;
			found = { nama: 'AHMAD BAHRUL MUGHDIQI', prodi: 'S1 — Manajemen' };
		}, 700);
	}

	function simpan(e: SubmitEvent) {
		e.preventDefault();
		window.location.href = '/admin/peserta-tes';
	}
</script>

<svelte:head>
	<title>Tambah Peserta — P2BA UNIA</title>
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
			<section class="card form-card">
				<a href="/admin/peserta-tes" class="back">← Kembali</a>
				<h2>Formulir Peserta Baru</h2>
				<p class="sub">Isi data peserta dengan lengkap dan benar.</p>

				<form onsubmit={simpan}>
					<span class="flabel">Tipe Peserta</span>
					<div class="tipe-grid" role="radiogroup" aria-label="Tipe Peserta">
						<button
							type="button"
							role="radio"
							aria-checked={tipe === 'internal'}
							class="tipe {tipe === 'internal' ? 'active' : ''}"
							onclick={() => (tipe = 'internal')}
						>
							<i class="fa-solid fa-building-columns"></i> Internal (UNIA)
						</button>
						<button
							type="button"
							role="radio"
							aria-checked={tipe === 'eksternal'}
							class="tipe {tipe === 'eksternal' ? 'active' : ''}"
							onclick={() => (tipe = 'eksternal')}
						>
							<i class="fa-solid fa-circle-user"></i> Eksternal / Umum
						</button>
					</div>

					{#if tipe === 'internal'}
						<div class="infobox">
							<i class="fa-regular fa-circle-question"></i>
							<p>Masukkan NIM Mahasiswa untuk mengambil data otomatis dari SIAKAD.</p>
						</div>

						<label class="field">
							<span>NIM Mahasiswa</span>
							<div class="nim-row">
								<input type="text" placeholder="Contoh: 20210001" bind:value={nim} />
								<button type="button" class="btn-primary" onclick={cekData} disabled={checking}>
									{checking ? 'Mengecek...' : 'Cek Data'}
								</button>
							</div>
						</label>

						{#if found}
							<div class="found">
								<i class="fa-solid fa-circle-check"></i>
								<div>
									<strong>{found.nama}</strong>
									<p>{found.prodi} · NIM {nim}</p>
								</div>
							</div>
						{/if}
					{:else}
						<div class="infobox">
							<i class="fa-regular fa-circle-question"></i>
							<p>Lengkapi data diri peserta eksternal / umum di bawah ini.</p>
						</div>

						<label class="field">
							<span>Nama Lengkap</span>
							<input type="text" placeholder="Contoh: Siti Aminah" bind:value={nama} required />
						</label>
						<div class="grid2">
							<label class="field">
								<span>Email</span>
								<input
									type="email"
									placeholder="nama@email.com"
									bind:value={email}
									required
								/>
							</label>
							<label class="field">
								<span>Institusi / Asal</span>
								<input
									type="text"
									placeholder="Contoh: USIM / Umum"
									bind:value={institusi}
									required
								/>
							</label>
						</div>
					{/if}

					<hr class="sep" />

					<h3>Daftarkan ke Jadwal Ujian</h3>
					<div class="infobox">
						<i class="fa-regular fa-circle-question"></i>
						<p>
							Peserta akan langsung terdaftar ke jadwal ujian yang dipilih. Pembayaran akan
							ditambahkan dengan status <strong>pending</strong> untuk diverifikasi nanti.
						</p>
					</div>

					<label class="field">
						<span>Jadwal Ujian</span>
						<select bind:value={jadwal} required>
							<option value="" disabled selected>Pilih jadwal ujian...</option>
							{#each jadwals as j}
								<option value={j}>{j}</option>
							{/each}
						</select>
					</label>

					<div class="submit-row">
						<button type="submit" class="btn-primary big">Simpan Peserta</button>
					</div>
				</form>
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
		--primary-light: #eff6ff;
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

	.container { padding: 20px 24px 28px; }
	.form-card { background: var(--bg-card); border-radius: var(--radius); box-shadow: var(--shadow-sm); padding: 24px 28px 28px; max-width: 860px; }
	.back { font-size: 13px; color: var(--text-muted); font-weight: 500; }
	.back:hover { color: var(--primary); }
	.form-card h2 { font-size: 20px; font-weight: 800; margin-top: 12px; }
	.sub { font-size: 12.5px; color: var(--text-muted); margin: 4px 0 18px; }
	.flabel { font-size: 12.5px; font-weight: 700; display: block; margin-bottom: 8px; }
	.tipe-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 16px; }
	.tipe { border: 1px solid #e2e8f0; background: #fff; border-radius: 12px; padding: 16px; font-size: 13.5px; font-weight: 600; color: var(--text-muted); cursor: pointer; display: flex; gap: 8px; align-items: center; justify-content: center; transition: all 0.2s; }
	.tipe i { font-size: 16px; }
	.tipe:hover { border-color: var(--primary); }
	.tipe.active { border-color: var(--primary); background: var(--primary-light); color: var(--primary-dark); box-shadow: 0 0 0 1px var(--primary); }
	.infobox { display: flex; gap: 10px; background: var(--primary-light); border-left: 3px solid var(--primary); border-radius: 0 10px 10px 0; padding: 12px 14px; font-size: 12.5px; color: #1e40af; margin-bottom: 16px; }
	.infobox i { margin-top: 2px; }
	.field { display: block; margin-bottom: 14px; }
	.field > span { font-size: 12.5px; font-weight: 700; display: block; margin-bottom: 8px; }
	.field input, .field select { width: 100%; padding: 11px 14px; border-radius: 10px; border: 1px solid #e2e8f0; font-size: 13.5px; outline: none; color: var(--text-dark); background: #f8fafc; }
	.field input:focus, .field select:focus { border-color: var(--primary); background: #fff; box-shadow: 0 0 0 3px var(--primary-light); }
	.nim-row { display: flex; gap: 10px; }
	.nim-row input { flex: 1; }
	.grid2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
	.found { display: flex; gap: 10px; align-items: center; background: #f0fdf4; border: 1px solid #bbf7d0; color: #15803d; border-radius: 10px; padding: 10px 14px; font-size: 13px; margin-bottom: 14px; }
	.found i { font-size: 18px; }
	.found p { font-size: 12px; }
	.sep { border: none; border-top: 1px solid var(--border-color); margin: 18px 0; }
	.form-card h3 { font-size: 14px; font-weight: 800; margin-bottom: 12px; }
	.btn-primary { background: var(--primary); color: #fff; border: none; border-radius: 10px; padding: 11px 22px; font-size: 13px; font-weight: 700; cursor: pointer; box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3); white-space: nowrap; }
	.btn-primary:hover { background: var(--primary-dark); }
	.btn-primary:disabled { opacity: 0.6; cursor: wait; }
	.submit-row { display: flex; justify-content: flex-end; margin-top: 18px; }
	.btn-primary.big { padding: 12px 30px; font-size: 14px; }

	@media (max-width: 768px) {
		.page { flex-direction: column; }
		.sidebar { width: 100%; position: static; height: auto; }
		.tipe-grid, .grid2 { grid-template-columns: 1fr; }
		.nim-row { flex-direction: column; }
	}
</style>
