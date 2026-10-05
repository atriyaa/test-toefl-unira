<script lang="ts">
	type Soal = {
		id: number;
		isi: string;
		kategori: string;
		kesulitan: 'Mudah' | 'Sedang' | 'Sulit';
		media: 'audio' | 'gambar' | 'teks' | null;
		status: 'Aktif' | 'Nonaktif';
	};

	const tarakibSamples = [
		'كان الجو .......... اليوم',
		'ذهب الطالب .......... المدرسة مبكرا',
		'إن المعلم .......... الدرس بوضوح',
		'الطالبان .......... في المكتبة',
		'نحن .......... اللغة العربية',
		'ذهبت .......... إلى السوق',
		'المسلم .......... الصلوات الخمس',
		'العلم .......... من المال',
		'فاطمة .......... القرآن كل يوم',
		'المعلمون .......... مخلصون'
	];

	const structureSamples = [
		'She _____ to school every day.',
		'Neither of the answers _____ correct.',
		'The committee _____ decided on the proposal.',
		'_____ the rain, the match continued.',
		'Had I known, I _____ come earlier.',
		'The book, _____ cover is torn, is mine.',
		'Not only _____ late, but he forgot the documents.',
		'By next month, she _____ finished her thesis.'
	];

	const readingSamples = [
		'According to the passage, the main idea of paragraph 2 is _____',
		'The word "phenomenon" in line 5 is closest in meaning to _____',
		'It can be inferred from the passage that _____',
		'The author mentions the experiment in order to _____',
		'Which of the following is NOT stated in the passage?'
	];

	const qiraahSamples = [
		'الفكرة الرئيسية في الفقرة الثانية ..........',
		'المراد بكلمة "الظاهرة" في السطر الخامس ..........',
		'يُفهم من النص أن ..........',
		'ذكر الكاتب التجربة من أجل ..........',
		'أي مما يلي غير مذكور في النص؟'
	];

	function buildData(): Soal[] {
		const out: Soal[] = [];
		let id = 1;
		const push = (isi: string, kategori: string, i: number) => {
			const kesulitan: Soal['kesulitan'] =
				i % 7 === 3 ? 'Sulit' : i % 5 === 2 ? 'Sedang' : 'Mudah';
			const status: Soal['status'] = id % 11 === 0 ? 'Nonaktif' : 'Aktif';
			const media: Soal['media'] =
				kategori === 'Reading' || kategori === 'Qiraah'
					? id % 4 === 0
						? 'teks'
						: null
					: null;
			out.push({ id: id++, isi, kategori, kesulitan, media, status });
		};
		for (let i = 0; i < 40; i++) push(structureSamples[i % structureSamples.length], 'Structure', i);
		for (let i = 0; i < 40; i++) push(tarakibSamples[i % tarakibSamples.length], 'Tarakib', i);
		for (let i = 0; i < 50; i++) push(readingSamples[i % readingSamples.length], 'Reading', i);
		for (let i = 0; i < 50; i++) push(qiraahSamples[i % qiraahSamples.length], 'Qiraah', i);
		return out;
	}

	const allData: Soal[] = buildData();

	const kategoriOptions = ['Semua Kategori', 'Listening', 'Istima', 'Structure', 'Tarakib', 'Reading', 'Qiraah'];

	let fKategori = $state('Semua Kategori');
	let fKesulitan = $state('Semua');
	let fStatus = $state('Semua');
	let fCari = $state('');

	let sKategori = $state('Semua Kategori');
	let sKesulitan = $state('Semua');
	let sStatus = $state('Semua');
	let sCari = $state('');

	let page = $state(1);
	const perPage = 15;
	let selected = $state<Set<number>>(new Set());

	let filtered = $derived(
		allData.filter((s) => {
			if (sKategori !== 'Semua Kategori' && s.kategori !== sKategori) return false;
			if (sKesulitan !== 'Semua' && s.kesulitan !== sKesulitan) return false;
			if (sStatus !== 'Semua' && s.status !== sStatus) return false;
			const q = sCari.trim().toLowerCase();
			if (q && !s.isi.toLowerCase().includes(q)) return false;
			return true;
		})
	);

	let totalPages = $derived(Math.max(1, Math.ceil(filtered.length / perPage)));
	let safePage = $derived(Math.min(page, totalPages));
	let startIdx = $derived((safePage - 1) * perPage);
	let rows = $derived(filtered.slice(startIdx, startIdx + perPage));
	let from = $derived(filtered.length === 0 ? 0 : startIdx + 1);
	let to = $derived(Math.min(startIdx + perPage, filtered.length));

	let countBy = $derived((kat: string) => allData.filter((s) => s.kategori === kat).length);

	function applyFilter() {
		sKategori = fKategori;
		sKesulitan = fKesulitan;
		sStatus = fStatus;
		sCari = fCari;
		page = 1;
		selected = new Set();
	}
	function resetFilter() {
		fKategori = 'Semua Kategori';
		fKesulitan = 'Semua';
		fStatus = 'Semua';
		fCari = '';
		applyFilter();
	}

	let pageNumbers = $derived(() => {
		const total = totalPages;
		const cur = safePage;
		let start = Math.max(1, Math.min(cur - 5, total - 11));
		let end = Math.min(total, start + 11);
		if (end - start < 11) start = Math.max(1, end - 11);
		return Array.from({ length: end - start + 1 }, (_, i) => start + i);
	});

	function toggleOne(id: number) {
		const next = new Set(selected);
		if (next.has(id)) next.delete(id);
		else next.add(id);
		selected = next;
	}
	function toggleAll(e: Event) {
		const checked = (e.target as HTMLInputElement).checked;
		if (checked) selected = new Set(rows.map((r) => r.id));
		else selected = new Set();
	}
	let allChecked = $derived(rows.length > 0 && rows.every((r) => selected.has(r.id)));

	function hapus(id: number) {
		if (!confirm('Hapus soal ini?')) return;
		const idx = allData.findIndex((s) => s.id === id);
		if (idx >= 0) allData.splice(idx, 1);
		const next = new Set(selected);
		next.delete(id);
		selected = next;
	}
</script>

<svelte:head>
	<title>Manajemen Bank Soal — P2BA UNIA</title>
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
				<h1>Manajemen Bank Soal</h1>
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
			<div class="toolbar">
				<p>Kelola Soal</p>
				<div class="toolbar-btns">
					<button class="btn-import"><i class="fa-solid fa-upload"></i> Import</button>
					<a href="/admin/bank-soal/tambah" class="btn-primary"
						><i class="fa-solid fa-plus"></i> Tambah Soal</a
					>
				</div>
			</div>

			<section class="card filter-card">
				<div class="filter-grid">
					<label><span>Kategori</span>
						<select bind:value={fKategori}>
							{#each kategoriOptions as k}<option>{k}</option>{/each}
						</select>
					</label>
					<label><span>Tingkat Kesulitan</span>
						<select bind:value={fKesulitan}>
							<option>Semua</option><option>Mudah</option><option>Sedang</option><option>Sulit</option>
						</select>
					</label>
					<label><span>Status</span>
						<select bind:value={fStatus}>
							<option>Semua</option><option>Aktif</option><option>Nonaktif</option>
						</select>
					</label>
					<label><span>Cari</span>
						<input type="text" placeholder="Cari isi soal..." bind:value={fCari} onkeydown={(e) => e.key === 'Enter' && applyFilter()} />
					</label>
					<div class="filter-btns">
						<button class="btn-primary" onclick={applyFilter}><i class="fa-solid fa-filter"></i> Filter</button>
						<button class="btn-gray" onclick={resetFilter}>Reset</button>
					</div>
				</div>
			</section>

			<section class="stats-grid">
				<div class="stat-card">
					<div class="stat-icon blue"><i class="fa-solid fa-list-ul"></i></div>
					<div><span>Total Soal</span><h3>{allData.length}</h3></div>
				</div>
				<div class="stat-card">
					<div class="stat-icon purple"><i class="fa-regular fa-folder"></i></div>
					<div><span>Listening</span><h3>{countBy('Listening')}</h3></div>
				</div>
				<div class="stat-card">
					<div class="stat-icon purple"><i class="fa-regular fa-folder"></i></div>
					<div><span>Istima</span><h3>{countBy('Istima')}</h3></div>
				</div>
				<div class="stat-card">
					<div class="stat-icon purple"><i class="fa-regular fa-folder"></i></div>
					<div><span>Structure</span><h3>{countBy('Structure')}</h3></div>
				</div>
				<div class="stat-card">
					<div class="stat-icon purple"><i class="fa-regular fa-folder"></i></div>
					<div><span>Tarakib</span><h3>{countBy('Tarakib')}</h3></div>
				</div>
				<div class="stat-card">
					<div class="stat-icon purple"><i class="fa-regular fa-folder"></i></div>
					<div><span>Reading</span><h3>{countBy('Reading')}</h3></div>
				</div>
				<div class="stat-card">
					<div class="stat-icon purple"><i class="fa-regular fa-folder"></i></div>
					<div><span>Qiraah</span><h3>{countBy('Qiraah')}</h3></div>
				</div>
			</section>

			<section class="card table-card">
				<div class="table-responsive">
					<table>
						<thead>
							<tr>
								<th class="center" style="width:44px">
									<input type="checkbox" checked={allChecked} onchange={toggleAll} aria-label="Pilih semua" />
								</th>
								<th style="width:60px">NO.</th>
								<th>SOAL</th>
								<th class="center">KATEGORI</th>
								<th class="center">KESULITAN</th>
								<th class="center">MEDIA</th>
								<th class="center">STATUS</th>
								<th class="right">AKSI</th>
							</tr>
						</thead>
						<tbody>
							{#each rows as s, i (s.id)}
								<tr>
									<td class="center">
										<input type="checkbox" checked={selected.has(s.id)} onchange={() => toggleOne(s.id)} aria-label="Pilih soal {s.id}" />
									</td>
									<td class="no">{startIdx + i + 1}</td>
									<td class="isi">{s.isi}</td>
									<td class="center"><span class="pill kat">{s.kategori}</span></td>
									<td class="center">
										{#if s.kesulitan === 'Mudah'}
											<span class="pill green">Mudah</span>
										{:else if s.kesulitan === 'Sedang'}
											<span class="pill yellow">Sedang</span>
										{:else}
											<span class="pill red">Sulit</span>
										{/if}
									</td>
									<td class="center media">
										{#if s.media === 'audio'}<i class="fa-solid fa-volume-high"></i>
										{:else if s.media === 'gambar'}<i class="fa-regular fa-image"></i>
										{:else if s.media === 'teks'}<i class="fa-regular fa-file-lines"></i>
										{:else}–{/if}
									</td>
									<td class="center">
										{#if s.status === 'Aktif'}
											<span class="pill green"><span class="dot"></span> Aktif</span>
										{:else}
											<span class="pill gray"><span class="dot gray"></span> Nonaktif</span>
										{/if}
									</td>
									<td class="right actions">
										<button title="Lihat" aria-label="Lihat"><i class="fa-regular fa-eye"></i></button>
										<button title="Edit" aria-label="Edit"><i class="fa-regular fa-pen-to-square"></i></button>
										<button title="Hapus" aria-label="Hapus" onclick={() => hapus(s.id)}>
											<i class="fa-regular fa-trash-can"></i>
										</button>
									</td>
								</tr>
							{/each}
							{#if rows.length === 0}
								<tr><td colspan="8" class="empty">Tidak ada soal yang cocok dengan filter.</td></tr>
							{/if}
						</tbody>
					</table>
				</div>
			</section>

			<div class="pager-row">
				<p class="info">Showing {from} to {to} of {filtered.length} results</p>
				<div class="pager">
					<button class="pg" disabled={safePage <= 1} onclick={() => (page = Math.max(1, safePage - 1))} aria-label="Sebelumnya">‹</button>
					{#each pageNumbers() as n}
						<button class="pg {n === safePage ? 'active' : ''}" onclick={() => (page = n)}>{n}</button>
					{/each}
					<button class="pg" disabled={safePage >= totalPages} onclick={() => (page = Math.min(totalPages, safePage + 1))} aria-label="Berikutnya">›</button>
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

	.container { padding: 20px 24px 28px; display: flex; flex-direction: column; gap: 16px; }
	.toolbar { display: flex; justify-content: space-between; align-items: center; }
	.toolbar p { font-size: 13.5px; color: #475569; }
	.toolbar-btns { display: flex; gap: 10px; }
	.btn-primary { background: var(--primary); color: #fff; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 700; cursor: pointer; display: inline-flex; gap: 8px; align-items: center; box-shadow: 0 4px 12px rgba(59,130,246,0.3); }
	.btn-primary:hover { background: var(--primary-dark); }
	.btn-import { background: var(--primary-dark); color: #fff; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 700; cursor: pointer; display: inline-flex; gap: 8px; align-items: center; box-shadow: 0 4px 12px rgba(37,99,235,0.3); }
	.btn-import:hover { filter: brightness(0.95); }
	.btn-gray { background: #eef2f7; color: #334155; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 600; cursor: pointer; }

	.card { background: var(--bg-card); border-radius: var(--radius); box-shadow: var(--shadow-sm); }
	.filter-card { padding: 18px 20px; }
	.filter-grid { display: grid; grid-template-columns: 1fr 1fr 1fr 1.4fr auto; gap: 12px; align-items: end; }
	.filter-grid label span { font-size: 12.5px; font-weight: 700; display: block; margin-bottom: 7px; color: #1e293b; }
	.filter-grid input, .filter-grid select { width: 100%; padding: 10px 14px; border-radius: 10px; border: 1px solid var(--input-border); font-size: 13px; outline: none; background: #fff; color: var(--text-dark); }
	.filter-grid input::placeholder { color: #9aa5b5; }
	.filter-grid input:focus, .filter-grid select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px var(--primary-light); }
	.filter-btns { display: flex; gap: 8px; }

	.stats-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 14px; }
	.stat-card { background: #fff; border-radius: var(--radius); box-shadow: var(--shadow-sm); padding: 16px 18px; display: flex; align-items: center; gap: 14px; }
	.stat-icon { width: 46px; height: 46px; border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 18px; flex-shrink: 0; }
	.stat-icon.blue { background: var(--primary-light); color: var(--primary-dark); }
	.stat-icon.purple { background: #f5f3ff; color: #7c3aed; }
	.stat-card span { font-size: 12px; color: var(--text-muted); }
	.stat-card h3 { font-size: 21px; font-weight: 800; margin-top: 2px; }

	.table-card { padding: 0; overflow: hidden; }
	.table-responsive { overflow-x: auto; }
	table { width: 100%; border-collapse: collapse; font-size: 13px; }
	th { text-align: left; padding: 12px 14px; color: #64748b; font-size: 11px; letter-spacing: 0.5px; border-bottom: 1px solid var(--border-color); background: #fbfcfe; white-space: nowrap; }
	td { padding: 13px 14px; border-bottom: 1px solid var(--border-color); vertical-align: middle; background: #fff; }
	tbody tr:last-child td { border-bottom: none; }
	tbody tr:hover td { background: #f8fafc; }
	input[type='checkbox'] { width: 16px; height: 16px; accent-color: var(--primary); cursor: pointer; }
	.no { color: #64748b; }
	.isi { color: #334155; max-width: 340px; }
	.media { color: #94a3b8; }
	.right { text-align: right; }
	.center { text-align: center; }
	.pill { display: inline-flex; align-items: center; gap: 5px; font-size: 11.5px; font-weight: 700; padding: 4px 11px; border-radius: 99px; white-space: nowrap; }
	.pill.kat { background: #f1f5f9; color: #475569; }
	.pill.green { background: #dcfce7; color: #15803d; }
	.pill.yellow { background: #fef9c3; color: #a16207; }
	.pill.red { background: #fee2e2; color: #b91c1c; }
	.pill.gray { background: #f1f5f9; color: #64748b; }
	.dot { width: 7px; height: 7px; border-radius: 50%; background: #22c55e; display: inline-block; }
	.dot.gray { background: #94a3b8; }
	.actions { white-space: nowrap; }
	.actions button { background: none; border: none; color: #64748b; cursor: pointer; padding: 7px; border-radius: 8px; font-size: 14px; }
	.actions button:hover { background: var(--primary-light); color: var(--primary); }
	.empty { text-align: center; color: var(--text-muted); padding: 28px; }

	.pager-row { display: flex; justify-content: space-between; align-items: center; gap: 12px; flex-wrap: wrap; }
	.info { font-size: 12.5px; color: #475569; }
	.pager { display: flex; align-items: center; background: #fff; border: 1px solid #e2e8f0; border-radius: 10px; overflow: hidden; }
	.pg { min-width: 38px; height: 36px; padding: 0 10px; border: none; border-left: 1px solid #e2e8f0; background: #fff; font-size: 13px; font-weight: 600; color: #475569; cursor: pointer; }
	.pg:first-child { border-left: none; }
	.pg:hover:not(:disabled):not(.active) { background: var(--primary-light); color: var(--primary-dark); }
	.pg.active { background: var(--primary); color: #fff; font-weight: 800; }
	.pg:disabled { opacity: 0.4; cursor: not-allowed; }

	@media (max-width: 1200px) { .stats-grid { grid-template-columns: repeat(2, 1fr); } .filter-grid { grid-template-columns: 1fr 1fr; } }
	@media (max-width: 768px) {
		.page { flex-direction: column; }
		.sidebar { width: 100%; position: static; height: auto; }
		.filter-grid { grid-template-columns: 1fr; }
		.toolbar { flex-direction: column; gap: 10px; align-items: stretch; }
		.stats-grid { grid-template-columns: 1fr; }
		.pager-row { flex-direction: column; align-items: stretch; }
		.pager { overflow-x: auto; }
	}
</style>
