<script lang="ts">
	import { goto } from '$app/navigation';

	let kategori = $state('');
	let kesulitan = $state('Sedang');
	let passage = $state('mandiri');
	let isi = $state('');
	let jawaban = $state('');
	let bobot: number | null = $state(1);
	let aktif = $state(true);

	let opsiTeks = $state<string[]>(['', '', '', '']);
	let opsiFiles = $state<Array<File | null>>([null, null, null, null]);
	let audioFile = $state<File | null>(null);
	let gambarFile = $state<File | null>(null);
	let videoFile = $state<File | null>(null);

	let errKategori = $state('');
	let errIsi = $state('');
	let errJawaban = $state('');
	let errBobot = $state('');

	const huruf = ['A', 'B', 'C', 'D'];

	function fileName(f: File | null) {
		return f ? f.name : 'No file chosen';
	}

	function onFile(e: Event, cb: (f: File | null) => void) {
		const input = e.target as HTMLInputElement;
		cb(input.files && input.files[0] ? input.files[0] : null);
	}

	function batal() {
		goto('/admin/bank-soal');
	}

	function simpan() {
		errKategori = errIsi = errJawaban = errBobot = '';
		if (!kategori) errKategori = 'Kategori wajib dipilih';
		if (!isi.trim()) errIsi = 'Isi soal wajib diisi';
		if (!jawaban) errJawaban = 'Jawaban benar wajib dipilih';
		const b = Number(bobot);
		if (bobot === null || isNaN(b)) errBobot = 'Bobot wajib diisi';
		else if (b < 1 || b > 10) errBobot = 'Bobot harus 1–10';
		if (errKategori || errIsi || errJawaban || errBobot) {
			document.querySelector('.form-card')?.scrollIntoView({ behavior: 'smooth' });
			return;
		}
		alert(`Soal disimpan — ${kategori} / ${kesulitan} / Jawaban ${jawaban}.`);
		goto('/admin/bank-soal');
	}
</script>

<svelte:head>
	<title>Tambah Soal Baru — P2BA UNIA</title>
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
			<a href="/admin/kategori-soal" class="nav-item">
				<i class="fa-solid fa-table-cells-large"></i><span>Kategori Soal</span>
			</a>
			<a href="/admin/bank-soal" class="nav-item active">
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
				<h1>Tambah Soal Baru</h1>
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
				<div class="grid2">
					<label class="field">
						<span>Kategori Soal <b>*</b></span>
						<select bind:value={kategori} class:invalid={!!errKategori}>
							<option value="" disabled selected>Pilih Kategori</option>
							<option>Listening</option><option>Istima</option><option>Structure</option>
							<option>Tarakib</option><option>Reading</option><option>Qiraah</option>
						</select>
						{#if errKategori}<small class="err">{errKategori}</small>{/if}
					</label>
					<label class="field">
						<span>Tingkat Kesulitan <b>*</b></span>
						<select bind:value={kesulitan}>
							<option>Mudah</option><option>Sedang</option><option>Sulit</option>
						</select>
					</label>
				</div>

				<label class="field">
					<span>Passage/Teks <em class="opt">(Opsional)</em></span>
					<select bind:value={passage}>
						<option value="mandiri">-- Soal Mandiri (Tanpa Passage) --</option>
						<option value="p1">Passage 1 — TOEFL Reading Practice</option>
						<option value="p2">Passage 2 — Teks Qiraah Dasar</option>
					</select>
					<small class="hint">Pilih passage jika soal ini terkait dengan teks bacaan yang sama.</small>
				</label>

				<label class="field">
					<span>Isi Soal <b>*</b></span>
					<textarea rows="5" placeholder="Tulis isi soal di sini..." bind:value={isi} class:invalid={!!errIsi}></textarea>
					{#if errIsi}<small class="err">{errIsi}</small>{/if}
				</label>

				<hr />

				<div class="media-title">Media Pendukung Soal <em class="opt">(Opsional)</em></div>
				<div class="grid3">
					<div class="field">
						<span class="media-label"><i class="fa-solid fa-volume-high blue"></i> Audio</span>
						<label class="filebox">
							<span class="choose">Choose File</span>
							<span class="fname">{fileName(audioFile)}</span>
							<input type="file" accept="audio/*" hidden onchange={(e) => onFile(e, (f) => (audioFile = f))} />
						</label>
						<small class="hint">MP3, WAV (Max: 10MB)</small>
					</div>
					<div class="field">
						<span class="media-label"><i class="fa-regular fa-image purple"></i> Gambar</span>
						<label class="filebox">
							<span class="choose">Choose File</span>
							<span class="fname">{fileName(gambarFile)}</span>
							<input type="file" accept="image/*" hidden onchange={(e) => onFile(e, (f) => (gambarFile = f))} />
						</label>
						<small class="hint">JPG, PNG, GIF (Max: 2MB)</small>
					</div>
					<div class="field">
						<span class="media-label"><i class="fa-regular fa-circle-play red"></i> Video</span>
						<label class="filebox">
							<span class="choose">Choose File</span>
							<span class="fname">{fileName(videoFile)}</span>
							<input type="file" accept="video/*" hidden onchange={(e) => onFile(e, (f) => (videoFile = f))} />
						</label>
						<small class="hint">MP4, WebM (Max: 50MB)</small>
					</div>
				</div>

				<div class="opsi-head">
					<span class="opsi-title">Pilihan Jawaban <b>*</b></span>
					<small class="hint">Setiap opsi bisa berupa teks atau file (gambar/audio/video)</small>
				</div>

				{#each huruf as h, i}
					<div class="opsi-card">
						<span class="badge">{h}</span>
						<div class="opsi-body">
							<input
								type="text"
								placeholder="Teks pilihan {h} (opsional jika ada file)"
								bind:value={opsiTeks[i]}
							/>
							<div class="opsi-file">
								<span>atau upload file:</span>
								<label class="choose-soft">
									Choose File
									<input type="file" hidden onchange={(e) => onFile(e, (f) => (opsiFiles[i] = f))} />
								</label>
								<span class="fname">{fileName(opsiFiles[i])}</span>
							</div>
						</div>
					</div>
				{/each}

				<div class="grid2">
					<label class="field">
						<span>Jawaban Benar <b>*</b></span>
						<select bind:value={jawaban} class:invalid={!!errJawaban}>
							<option value="" disabled selected>Pilih Jawaban</option>
							<option value="A">A</option><option value="B">B</option>
							<option value="C">C</option><option value="D">D</option>
						</select>
						{#if errJawaban}<small class="err">{errJawaban}</small>{/if}
					</label>
					<label class="field">
						<span>Bobot Nilai</span>
						<input type="number" min="1" max="10" bind:value={bobot} class:invalid={!!errBobot} />
						{#if errBobot}
							<small class="err">{errBobot}</small>
						{:else}
							<small class="hint">Bobot nilai soal (1–10)</small>
						{/if}
					</label>
				</div>

				<label class="check-row">
					<input type="checkbox" bind:checked={aktif} />
					<span><strong>Soal Aktif</strong></span>
				</label>

				<hr />

				<div class="actions">
					<button class="btn-gray" onclick={batal}>Batal</button>
					<button class="btn-primary" onclick={simpan}>
						<i class="fa-regular fa-floppy-disk"></i> Simpan Soal
					</button>
				</div>
			</div>

			<footer class="foot">© 2026 P2BA UNIA Administration System v1.0</footer>
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

	.container { padding: 24px 20px 20px; display: flex; flex-direction: column; align-items: center; gap: 16px; }
	.form-card { background: #fff; border-radius: var(--radius); box-shadow: var(--shadow-sm); width: 100%; max-width: 860px; padding: 30px 34px 24px; display: flex; flex-direction: column; gap: 18px; }
	.grid2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
	.grid3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 14px; }
	.field { display: flex; flex-direction: column; gap: 7px; }
	.field span { font-size: 13px; font-weight: 700; color: #1e293b; }
	.field span b { color: #ef4444; }
	.opt { color: var(--text-muted); font-weight: 500; font-style: normal; }
	.field input, .field textarea, .field select {
		width: 100%; padding: 11px 15px; border-radius: 10px; border: 1px solid var(--input-border);
		font-size: 13.5px; outline: none; background: #fff; color: var(--text-dark);
	}
	.field input::placeholder, .field textarea::placeholder { color: #9aa5b5; }
	.field textarea { resize: vertical; min-height: 120px; }
	.field input:focus, .field textarea:focus, .field select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px var(--primary-light); }
	.field .invalid { border-color: #ef4444; box-shadow: 0 0 0 3px #fee2e2; }
	.hint { font-size: 12px; color: var(--text-muted); font-weight: 400; }
	.err { font-size: 12px; color: #dc2626; font-weight: 600; }
	hr { border: none; border-top: 1px solid var(--border-color); }

	.media-title { font-size: 14px; font-weight: 700; }
	.media-label { display: flex; align-items: center; gap: 6px; font-size: 13px; }
	.media-label .blue { color: var(--primary); }
	.media-label .purple { color: #7c3aed; }
	.media-label .red { color: #ef4444; }
	.filebox { display: flex; align-items: center; gap: 10px; border: 1px solid var(--input-border); border-radius: 10px; padding: 6px 12px 6px 6px; background: #fff; cursor: pointer; font-size: 12.5px; }
	.filebox:hover { border-color: var(--primary); }
	.choose { background: #f1f5f9; border: 1px solid #cbd5e1; border-radius: 6px; padding: 6px 12px; font-weight: 600; font-size: 12.5px; white-space: nowrap; }
	.fname { color: var(--text-muted); overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

	.opsi-head { display: flex; justify-content: space-between; align-items: center; gap: 12px; flex-wrap: wrap; }
	.opsi-title { font-size: 13px; font-weight: 700; }
	.opsi-title b { color: #ef4444; }
	.opsi-card { background: #f8fafc; border-radius: 12px; padding: 16px; display: flex; gap: 14px; align-items: flex-start; }
	.badge { width: 32px; height: 32px; border-radius: 8px; background: #d1fae5; color: #047857; font-weight: 800; font-size: 13px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
	.opsi-body { flex: 1; display: flex; flex-direction: column; gap: 10px; }
	.opsi-body input[type='text'] { width: 100%; padding: 11px 15px; border-radius: 10px; border: 1px solid var(--input-border); font-size: 13.5px; outline: none; background: #fff; }
	.opsi-body input[type='text']::placeholder { color: #9aa5b5; }
	.opsi-body input[type='text']:focus { border-color: var(--primary); box-shadow: 0 0 0 3px var(--primary-light); }
	.opsi-file { display: flex; align-items: center; gap: 10px; font-size: 12.5px; color: var(--text-muted); flex-wrap: wrap; }
	.choose-soft { background: #ecfdf5; color: #047857; border-radius: 8px; padding: 5px 12px; font-weight: 600; cursor: pointer; font-size: 12px; }
	.choose-soft:hover { background: #d1fae5; }

	.check-row { display: flex; gap: 10px; align-items: center; cursor: pointer; }
	.check-row input { width: 18px; height: 18px; accent-color: var(--primary); cursor: pointer; }
	.check-row strong { font-size: 13.5px; }

	.actions { display: flex; justify-content: flex-end; gap: 12px; }
	.btn-gray { background: #fff; color: #334155; border: 1px solid var(--input-border); border-radius: 10px; padding: 11px 24px; font-size: 13.5px; font-weight: 600; cursor: pointer; }
	.btn-gray:hover { background: #f8fafc; }
	.btn-primary { background: var(--primary); color: #fff; border: none; border-radius: 10px; padding: 11px 22px; font-size: 13.5px; font-weight: 700; cursor: pointer; display: inline-flex; gap: 8px; align-items: center; box-shadow: 0 4px 12px rgba(59,130,246,0.3); }
	.btn-primary:hover { background: var(--primary-dark); }

	.foot { font-size: 12px; color: #94a3b8; text-align: center; padding: 8px 0 12px; border-top: 1px solid #eef2f7; width: 100%; }

	@media (max-width: 768px) {
		.page { flex-direction: column; }
		.sidebar { width: 100%; position: static; height: auto; }
		.form-card { padding: 20px 18px; }
		.grid2, .grid3 { grid-template-columns: 1fr; }
		.actions { flex-direction: column-reverse; }
		.actions button { width: 100%; justify-content: center; }
	}
</style>
