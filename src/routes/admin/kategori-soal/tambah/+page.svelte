<script lang="ts">
	import { goto } from '$app/navigation';

	let nama = $state('');
	let deskripsi = $state('');
	let durasi: number | null = $state(35);
	let aktif = $state(true);
	let errorNama = $state('');
	let errorDurasi = $state('');

	function batal() {
		goto('/admin/kategori-soal');
	}

	function simpan() {
		errorNama = '';
		errorDurasi = '';
		if (!nama.trim()) {
			errorNama = 'Nama kategori wajib diisi';
		}
		const d = Number(durasi);
		if (durasi === null || durasi === undefined || isNaN(d)) {
			errorDurasi = 'Durasi wajib diisi';
		} else if (d < 1 || d > 180) {
			errorDurasi = 'Durasi harus antara 1–180 menit';
		}
		if (errorNama || errorDurasi) return;
		alert(`Kategori "${nama.trim()}" disimpan — ${d} menit, ${aktif ? 'Aktif' : 'Nonaktif'}.`);
		goto('/admin/kategori-soal');
	}
</script>

<svelte:head>
	<title>Tambah Kategori Soal — P2BA UNIA</title>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap"
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
			<a href="/admin/jadwal-ujian" class="nav-item">
				<i class="fa-regular fa-calendar-days"></i><span>Jadwal Ujian</span>
			</a>
			<a href="/admin/kategori-soal" class="nav-item active">
				<i class="fa-solid fa-table-cells-large"></i><span>Kategori Soal</span>
			</a>
			<a href="/admin/bank-soal" class="nav-item">
				<i class="fa-solid fa-book-open"></i><span>Bank Soal</span>
			</a>
			<a href="/admin/passage" class="nav-item">
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
				<h1>Tambah Kategori Soal</h1>
				<p>Overview data dan statistik</p>
			</div>
			<div class="header-right">
				<button class="notification-btn" aria-label="Notifications">
					<i class="fa-regular fa-bell"></i><span class="notification-badge"></span>
				</button>
				<div class="vdiv"></div>
				<a href="/login" class="logout-link"
					><i class="fa-solid fa-arrow-right-from-bracket"></i> Keluar</a
				>
			</div>
		</header>

		<div class="container">
			<div class="form-card">
				<label class="field">
					<span>Nama Kategori <b>*</b></span>
					<input
						type="text"
						placeholder="Contoh: Listening Comprehension"
						bind:value={nama}
						class:invalid={!!errorNama}
					/>
					{#if errorNama}<small class="err">{errorNama}</small>{/if}
				</label>

				<label class="field">
					<span>Deskripsi</span>
					<textarea rows="4" placeholder="Deskripsi singkat tentang kategori soal" bind:value={deskripsi}></textarea>
				</label>

				<label class="field">
					<span>Durasi Waktu (Menit) <b>*</b></span>
					<div class="input-suffix" class:invalid={!!errorDurasi}>
						<input type="number" min="1" max="180" bind:value={durasi} />
						<em>menit</em>
					</div>
					{#if errorDurasi}
						<small class="err">{errorDurasi}</small>
					{:else}
						<small class="hint">Batas waktu pengerjaan untuk kategori ini (1–180 menit)</small>
					{/if}
				</label>

				<label class="check-row">
					<input type="checkbox" bind:checked={aktif} />
					<span>
						<strong>Kategori Aktif</strong>
						<small>Kategori yang tidak aktif tidak akan muncul dalam pilihan saat membuat jadwal ujian</small>
					</span>
				</label>

				<hr />

				<div class="actions">
					<button class="btn-gray" onclick={batal}>Batal</button>
					<button class="btn-primary" onclick={simpan}>
						<i class="fa-regular fa-floppy-disk"></i> Simpan Kategori
					</button>
				</div>
			</div>
		</div>
	</main>
</div>

<style>
	:root {
		--bg-main: #f7f8f7; --bg-card: #fff; --primary: #3b82f6; --primary-dark: #2563eb;
		--primary-light: #eff6ff; --primary-gradient: linear-gradient(135deg, #2563eb, #3b82f6);
		--text-dark: #1e293b; --text-muted: #64748b; --border-color: #f1f5f9;
		--shadow-sm: 0 4px 6px -1px rgba(0,0,0,0.03), 0 2px 4px -1px rgba(0,0,0,0.02);
		--radius: 14px; --input-border: #d7dde5;
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
	.notification-btn { position: relative; background: none; border: none; font-size: 19px; color: #94a3b8; cursor: pointer; }
	.notification-badge { position: absolute; top: -2px; right: -2px; background: #ef4444; width: 8px; height: 8px; border-radius: 50%; border: 2px solid #fff; }
	.vdiv { width: 1px; height: 24px; background: #e2e8f0; }
	.logout-link { font-size: 13px; font-weight: 600; color: #64748b; display: flex; gap: 8px; align-items: center; }

	.container { padding: 28px 20px 32px; display: flex; justify-content: center; }
	.form-card { background: #fff; border-radius: var(--radius); box-shadow: var(--shadow-sm); width: 100%; max-width: 660px; padding: 32px 36px 24px; display: flex; flex-direction: column; gap: 20px; }
	.field { display: flex; flex-direction: column; gap: 7px; }
	.field span { font-size: 13px; font-weight: 700; color: #1e293b; }
	.field span b { color: #ef4444; }
	.field input, .field textarea {
		width: 100%; padding: 12px 15px; border-radius: 10px; border: 1px solid var(--input-border);
		font-size: 13.5px; outline: none; background: #fff; color: var(--text-dark);
	}
	.field input::placeholder, .field textarea::placeholder { color: #9aa5b5; }
	.field textarea { resize: vertical; min-height: 96px; }
	.field input:focus, .field textarea:focus { border-color: var(--primary); box-shadow: 0 0 0 3px var(--primary-light); }
	.field input.invalid { border-color: #ef4444; box-shadow: 0 0 0 3px #fee2e2; }
	.input-suffix { position: relative; display: flex; align-items: center; }
	.input-suffix input { padding-right: 70px; }
	.input-suffix input::-webkit-outer-spin-button, .input-suffix input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
	.input-suffix input[type='number'] { -moz-appearance: textfield; appearance: textfield; }
	.input-suffix em { position: absolute; right: 16px; font-style: normal; font-size: 13px; color: #64748b; pointer-events: none; }
	.input-suffix.invalid input { border-color: #ef4444; box-shadow: 0 0 0 3px #fee2e2; }
	.hint { font-size: 12px; color: var(--text-muted); font-weight: 400; }
	.err { font-size: 12px; color: #dc2626; font-weight: 600; }

	.check-row { display: flex; gap: 12px; align-items: flex-start; cursor: pointer; }
	.check-row input { width: 19px; height: 19px; margin-top: 1px; accent-color: var(--primary); cursor: pointer; flex-shrink: 0; border-radius: 5px; }
	.check-row strong { font-size: 13.5px; display: block; color: #1e293b; }
	.check-row small { font-size: 12.5px; color: var(--text-muted); display: block; margin-top: 3px; }

	hr { border: none; border-top: 1px solid var(--border-color); }

	.actions { display: flex; justify-content: flex-end; gap: 12px; }
	.btn-gray { background: #fff; color: #334155; border: 1px solid var(--input-border); border-radius: 10px; padding: 11px 24px; font-size: 13.5px; font-weight: 600; cursor: pointer; }
	.btn-gray:hover { background: #f8fafc; }
	.btn-primary { background: var(--primary); color: #fff; border: none; border-radius: 10px; padding: 11px 22px; font-size: 13.5px; font-weight: 700; cursor: pointer; display: inline-flex; gap: 8px; align-items: center; box-shadow: 0 4px 12px rgba(59,130,246,0.3); }
	.btn-primary:hover { background: var(--primary-dark); }

	@media (max-width: 768px) {
		.page { flex-direction: column; }
		.sidebar { width: 100%; position: static; height: auto; }
		.form-card { padding: 22px 18px; }
		.actions { flex-direction: column-reverse; }
		.actions button { width: 100%; justify-content: center; }
	}
</style>
