<script lang="ts">
	import { goto } from '$app/navigation';

	type Kategori = {
		id: string;
		nama: string;
		tersedia: number;
		checked: boolean;
		jumlah: number;
		open: boolean;
	};

	let kategoris: Kategori[] = $state([
		{ id: 'listening', nama: 'Listening', tersedia: 0, checked: false, jumlah: 0, open: false },
		{ id: 'istima', nama: 'Istima', tersedia: 0, checked: false, jumlah: 0, open: false },
		{ id: 'structure', nama: 'Structure', tersedia: 40, checked: false, jumlah: 20, open: false },
		{ id: 'tarakib', nama: 'Tarakib', tersedia: 40, checked: false, jumlah: 20, open: false },
		{ id: 'reading', nama: 'Reading', tersedia: 50, checked: false, jumlah: 20, open: false },
		{ id: "qiraah", nama: "Qira'ah", tersedia: 50, checked: false, jumlah: 20, open: false }
	]);

	let judul = $state('');
	let deskripsi = $state('');
	let tanggal = $state('');
	let jamMulai = $state('08:00');
	let jamSelesai = $state('11:00');
	let mode = $state('Offline');
	let jenisTes = $state('TOEFL Prediction');
	let kuota = $state(50);
	let lokasi = $state('');
	let hargaInt = $state(75000);
	let hargaExt = $state(150000);
	let regBuka = $state('');
	let regTutup = $state('');
	let acakSoal = $state(false);
	let acakJawaban = $state(false);
	let statusAwal = $state('Draft');
	let konversiOpen = $state(false);

	let jmlDipilih = $derived(kategoris.filter((k) => k.checked).length);
	let totalSoal = $derived(kategoris.filter((k) => k.checked).reduce((a, k) => a + (Number(k.jumlah) || 0), 0));

	function toggleCheck(k: Kategori) {
		k.checked = !k.checked;
		if (k.checked && k.tersedia === 0) {
			// tetap boleh dicentang tapi jumlah 0
			k.jumlah = 0;
		}
		if (k.checked) k.open = true;
	}

	function toggleOpen(k: Kategori) {
		k.open = !k.open;
	}

	function clampJumlah(k: Kategori) {
		if (k.jumlah < 0) k.jumlah = 0;
		if (k.jumlah > k.tersedia) k.jumlah = k.tersedia;
	}

	function batal() {
		goto('/admin/jadwal-ujian');
	}

	function simpan() {
		if (!judul.trim()) {
			alert('Judul Jadwal wajib diisi');
			return;
		}
		if (!tanggal) {
			alert('Tanggal wajib diisi');
			return;
		}
		alert(`Jadwal "${judul}" disimpan sebagai ${statusAwal} — ${jmlDipilih} kategori, ${totalSoal} soal.`);
		goto('/admin/jadwal-ujian');
	}
</script>

<svelte:head>
	<title>Buat Jadwal Ujian — P2BA UNIA</title>
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
				<div class="breadcrumb"><a href="/admin/jadwal-ujian">Jadwal Ujian</a><span>/</span><span>Buat</span></div>
				<h1>Buat Jadwal Ujian</h1>
				<p>Overview data dan statistik</p>
			</div>
			<div class="header-right">
				<button class="notification-btn" aria-label="Notifications">
					<i class="fa-regular fa-bell"></i><span class="notification-badge"></span>
				</button>
			</div>
		</header>

		<div class="container">
			<div class="form-card">
				<!-- ===== INFORMASI DASAR ===== -->
				<section class="section">
					<h2 class="section-title">Informasi Dasar</h2>

					<label class="field">
						<span>Judul Jadwal <b>*</b></span>
						<input type="text" placeholder="Contoh: TOEFL Prediction Test Januari 2026" bind:value={judul} />
					</label>

					<label class="field">
						<span>Deskripsi</span>
						<textarea rows="3" placeholder="Deskripsi singkat tentang jadwal ujian" bind:value={deskripsi}></textarea>
					</label>

					<div class="grid-3">
						<label class="field">
							<span>Tanggal <b>*</b></span>
							<input type="date" bind:value={tanggal} />
						</label>
						<label class="field">
							<span>Waktu Mulai <b>*</b></span>
							<input type="time" bind:value={jamMulai} />
						</label>
						<label class="field">
							<span>Waktu Selesai <b>*</b></span>
							<input type="time" bind:value={jamSelesai} />
						</label>
					</div>

					<div class="grid-3">
						<label class="field">
							<span>Mode <b>*</b></span>
							<div class="select-wrap">
								<select bind:value={mode}>
									<option>Offline</option>
									<option>Online</option>
								</select>
								<i class="fa-solid fa-chevron-down"></i>
							</div>
						</label>
						<label class="field">
							<span>Jenis Tes <b>*</b></span>
							<div class="select-wrap">
								<select bind:value={jenisTes}>
									<option>TOEFL Prediction</option>
									<option>TOAFL Prediction</option>
									<option>TOEFL ITP</option>
									<option>TOAFL ITP</option>
								</select>
								<i class="fa-solid fa-chevron-down"></i>
							</div>
						</label>
						<label class="field">
							<span>Kuota <b>*</b></span>
							<input type="number" min="1" bind:value={kuota} />
						</label>
					</div>

					<label class="field">
						<span>Lokasi</span>
						<input type="text" placeholder="Contoh: Lab Bahasa Gedung A Lt. 2" bind:value={lokasi} />
					</label>

					<div class="grid-2">
						<label class="field">
							<span>Harga Internal (Rp) <b>*</b></span>
							<input type="number" min="0" bind:value={hargaInt} />
						</label>
						<label class="field">
							<span>Harga Eksternal (Rp) <b>*</b></span>
							<input type="number" min="0" bind:value={hargaExt} />
						</label>
					</div>

					<div class="grid-2">
						<label class="field">
							<span>Pendaftaran Dibuka</span>
							<input type="datetime-local" bind:value={regBuka} />
						</label>
						<label class="field">
							<span>Pendaftaran Ditutup</span>
							<input type="datetime-local" bind:value={regTutup} />
						</label>
					</div>
				</section>

				<hr />

				<!-- ===== KONFIGURASI KATEGORI ===== -->
				<section class="section">
					<h2 class="section-title">Konfigurasi Kategori Soal</h2>
					<p class="section-sub">Pilih kategori dan tentukan jumlah soal yang akan digunakan dari setiap kategori.</p>

					<div class="kat-list">
						{#each kategoris as k}
							<div class="kat-item" class:checked={k.checked}>
								<div class="kat-head">
									<label class="kat-left">
										<input
											type="checkbox"
											checked={k.checked}
											onchange={() => toggleCheck(k)}
											aria-label={k.nama}
										/>
										<strong>{k.nama}</strong>
										<span class="avail">({k.tersedia} soal tersedia)</span>
										{#if k.checked && k.jumlah > 0}
											<span class="picked">{k.jumlah} dipakai</span>
										{/if}
									</label>
									<button type="button" class="chev" onclick={() => toggleOpen(k)} aria-label="Buka {k.nama}">
										<i class="fa-solid fa-chevron-down" class:rot={k.open}></i>
									</button>
								</div>
								{#if k.open}
									<div class="kat-body">
										{#if k.tersedia === 0}
											<p class="empty-note"><i class="fa-solid fa-circle-exclamation"></i> Belum ada soal tersedia untuk kategori ini.</p>
										{:else}
											<div class="kat-config">
												<label>
													<span>Jumlah soal digunakan</span>
													<div class="num-row">
														<input
															type="number"
															min="0"
															max={k.tersedia}
															bind:value={k.jumlah}
															onchange={() => clampJumlah(k)}
															disabled={!k.checked}
														/>
														<span class="max">/ {k.tersedia} tersedia</span>
													</div>
												</label>
												<input
													type="range"
													min="0"
													max={k.tersedia}
													bind:value={k.jumlah}
													disabled={!k.checked}
													aria-label="Jumlah {k.nama}"
												/>
											</div>
										{/if}
									</div>
								{/if}
							</div>
						{/each}
					</div>

					<div class="summary">
						<div class="row"><span>Kategori dipilih:</span><strong>{jmlDipilih}</strong></div>
						<div class="row"><span>Total soal:</span><strong>{totalSoal}</strong></div>
					</div>
				</section>

				<hr />

				<!-- ===== PENGACAKAN ===== -->
				<section class="section">
					<h2 class="section-title">Pengaturan Pengacakan</h2>
					<label class="check-row">
						<input type="checkbox" bind:checked={acakSoal} />
						<span>
							<strong>Acak Urutan Soal</strong>
							<small>Setiap peserta akan mendapat urutan soal yang berbeda</small>
						</span>
					</label>
					<label class="check-row">
						<input type="checkbox" bind:checked={acakJawaban} />
						<span>
							<strong>Acak Urutan Jawaban</strong>
							<small>Pilihan jawaban A, B, C, D akan diacak untuk setiap peserta</small>
						</span>
					</label>
				</section>

				<hr />

				<section class="section">
					<label class="field">
						<span>Status Awal</span>
						<div class="select-wrap">
							<select bind:value={statusAwal}>
								<option>Draft</option>
								<option>Open</option>
								<option>Closed</option>
							</select>
							<i class="fa-solid fa-chevron-down"></i>
						</div>
					</label>
				</section>

				<hr />

				<section class="section">
					<button type="button" class="konv-head" onclick={() => (konversiOpen = !konversiOpen)}>
						<span class="konv-title">
							<strong>Tabel Konversi Nilai Standar (Fallback)</strong>
							<span class="ref-badge">Referensi</span>
						</span>
						<span class="konv-sub">Tabel ini akan digunakan jika tidak ada konversi nilai khusus yang diatur.</span>
						<i class="fa-solid fa-chevron-down" class:rot={konversiOpen}></i>
					</button>
					{#if konversiOpen}
						<div class="konv-body">
							<div class="table-wrap">
								<table>
									<thead><tr><th>Benar</th><th>Listening</th><th>Structure</th><th>Reading</th></tr></thead>
									<tbody>
										<tr><td>40 – 50</td><td>62 – 68</td><td>62 – 68</td><td>60 – 67</td></tr>
										<tr><td>30 – 39</td><td>54 – 61</td><td>53 – 61</td><td>52 – 59</td></tr>
										<tr><td>20 – 29</td><td>46 – 53</td><td>44 – 52</td><td>43 – 51</td></tr>
										<tr><td>0 – 19</td><td>31 – 45</td><td>31 – 43</td><td>31 – 42</td></tr>
									</tbody>
								</table>
							</div>
							<p class="hint"><i class="fa-regular fa-circle-question"></i> Nilai akhir = (Listening + Structure + Reading) × 10 / 3</p>
						</div>
					{/if}
				</section>

				<hr />

				<div class="actions">
					<button class="btn-gray" onclick={batal}>Batal</button>
					<button class="btn-primary" onclick={simpan}><i class="fa-regular fa-floppy-disk"></i> Simpan Jadwal</button>
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
	.breadcrumb { font-size: 11px; color: var(--text-muted); margin-bottom: 4px; display: flex; gap: 6px; }
	.breadcrumb a { color: var(--primary-dark); font-weight: 600; }
	.header-title h1 { font-size: 20px; font-weight: 800; }
	.header-title p { font-size: 12px; color: var(--text-muted); margin-top: 2px; }
	.header-right { display: flex; align-items: center; gap: 16px; }
	.notification-btn { position: relative; background: none; border: none; font-size: 19px; color: #94a3b8; cursor: pointer; }
	.notification-badge { position: absolute; top: -2px; right: -2px; background: #ef4444; width: 8px; height: 8px; border-radius: 50%; border: 2px solid #fff; }

	.container { padding: 18px 20px 28px; display: flex; justify-content: center; }
	.form-card { background: #fff; border-radius: var(--radius); box-shadow: var(--shadow-sm); width: 100%; max-width: 880px; padding: 26px 30px 22px; }
	.section { display: flex; flex-direction: column; gap: 16px; }
	.section-title { font-size: 15px; font-weight: 800; }
	.section-sub { font-size: 13px; color: var(--text-muted); margin-top: -10px; }
	hr { border: none; border-top: 1px solid var(--border-color); margin: 20px 0; }

	.field { display: flex; flex-direction: column; gap: 7px; }
	.field span { font-size: 13px; font-weight: 600; color: #334155; }
	.field span b { color: #ef4444; }
	.field input, .field textarea, .select-wrap select {
		width: 100%; padding: 11px 15px; border-radius: 10px; border: 1px solid var(--input-border);
		font-size: 13.5px; outline: none; background: #fff; color: var(--text-dark);
	}
	.field input::placeholder, .field textarea::placeholder { color: #9aa5b5; }
	.field textarea { resize: vertical; min-height: 84px; }
	.field input:focus, .field textarea:focus, .select-wrap select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px var(--primary-light); }
	.select-wrap { position: relative; }
	.select-wrap select { appearance: none; padding-right: 38px; cursor: pointer; }
	.select-wrap i { position: absolute; right: 14px; top: 50%; transform: translateY(-50%); font-size: 12px; color: #334155; pointer-events: none; }

	.grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 14px; }
	.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }

	.kat-list { display: flex; flex-direction: column; gap: 12px; }
	.kat-item { border: 1px solid #e6ebf1; border-radius: 10px; background: #fbfcfe; overflow: hidden; transition: all .15s; }
	.kat-item.checked { border-color: #bfdbfe; background: var(--primary-light); }
	.kat-head { width: 100%; display: flex; justify-content: space-between; align-items: center; padding: 14px 16px; }
	.kat-left { display: flex; align-items: center; gap: 10px; font-size: 13.5px; cursor: pointer; }
	.kat-left input { width: 17px; height: 17px; accent-color: var(--primary); cursor: pointer; }
	.kat-left strong { font-weight: 700; }
	.avail { color: var(--text-muted); font-weight: 400; font-size: 12.5px; }
	.picked { font-size: 11px; font-weight: 700; background: var(--primary); color: #fff; padding: 2px 10px; border-radius: 99px; }
	.chev { background: none; border: none; cursor: pointer; padding: 6px; }
	.chev i { font-size: 12px; color: #94a3b8; transition: transform .2s; }
	.chev i.rot { transform: rotate(180deg); }
	.kat-body { border-top: 1px solid #e6ebf1; padding: 14px 16px; background: #fff; }
	.empty-note { font-size: 12.5px; color: var(--text-muted); display: flex; gap: 8px; align-items: center; }
	.empty-note i { color: #f59e0b; }
	.kat-config { display: flex; flex-direction: column; gap: 10px; }
	.kat-config label span { font-size: 12px; font-weight: 600; color: #475569; display: block; margin-bottom: 6px; }
	.num-row { display: flex; align-items: center; gap: 10px; }
	.num-row input[type='number'] { width: 140px; padding: 9px 12px; border-radius: 10px; border: 1px solid var(--input-border); font-size: 13px; outline: none; }
	.num-row input:focus { border-color: var(--primary); box-shadow: 0 0 0 3px var(--primary-light); }
	.max { font-size: 12px; color: var(--text-muted); }
	input[type='range'] { width: 100%; accent-color: var(--primary); }

	.summary { background: var(--primary-light); border-radius: 10px; padding: 13px 18px; display: flex; flex-direction: column; gap: 5px; margin-top: 4px; }
	.summary .row { display: flex; justify-content: space-between; font-size: 13px; color: #1d4ed8; }
	.summary .row strong { font-weight: 800; }

	.check-row { display: flex; gap: 12px; align-items: flex-start; cursor: pointer; }
	.check-row input { width: 18px; height: 18px; margin-top: 2px; accent-color: var(--primary); cursor: pointer; flex-shrink: 0; }
	.check-row strong { font-size: 13.5px; display: block; }
	.check-row small { font-size: 12.5px; color: var(--text-muted); }

	.konv-head { background: none; border: none; width: 100%; text-align: left; cursor: pointer; display: flex; justify-content: space-between; align-items: flex-start; gap: 12px; }
	.konv-title { display: flex; align-items: center; gap: 10px; font-size: 14px; }
	.ref-badge { font-size: 11px; font-weight: 700; background: #dbeafe; color: #2563eb; padding: 3px 10px; border-radius: 99px; }
	.konv-sub { display: block; font-size: 12.5px; color: var(--text-muted); margin-top: 6px; font-weight: 400; }
	.konv-head i { font-size: 13px; color: #64748b; transition: transform .2s; margin-top: 4px; }
	.konv-head i.rot { transform: rotate(180deg); }
	.konv-body { margin-top: 14px; }
	.table-wrap { overflow-x: auto; border: 1px solid #e6ebf1; border-radius: 10px; }
	table { width: 100%; border-collapse: collapse; font-size: 12.5px; }
	th { background: #f8fafc; text-align: left; padding: 10px 12px; color: var(--text-muted); }
	td { padding: 10px 12px; border-top: 1px solid var(--border-color); }
	.hint { font-size: 12px; color: var(--text-muted); margin-top: 10px; display: flex; gap: 6px; align-items: center; }

	.actions { display: flex; justify-content: flex-end; gap: 12px; }
	.btn-gray { background: #fff; color: #334155; border: 1px solid var(--input-border); border-radius: 10px; padding: 11px 24px; font-size: 13.5px; font-weight: 600; cursor: pointer; }
	.btn-gray:hover { background: #f8fafc; }
	.btn-primary { background: var(--primary); color: #fff; border: none; border-radius: 10px; padding: 11px 22px; font-size: 13.5px; font-weight: 700; cursor: pointer; display: inline-flex; gap: 8px; align-items: center; box-shadow: 0 4px 12px rgba(59,130,246,0.3); }
	.btn-primary:hover { background: var(--primary-dark); }

	@media (max-width: 768px) {
		.page { flex-direction: column; }
		.sidebar { width: 100%; position: static; height: auto; }
		.form-card { padding: 20px 18px; }
		.grid-3, .grid-2 { grid-template-columns: 1fr; }
		.actions { flex-direction: column-reverse; }
		.actions button { width: 100%; justify-content: center; }
	}
</style>
