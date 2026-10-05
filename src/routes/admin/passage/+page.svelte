<script lang="ts">
	type Passage = {
		id: number;
		judul: string;
		isi: string;
		kategori: string;
		jumlah: number;
		media: string | null;
		status: 'Aktif' | 'Nonaktif';
	};

	const base: Passage[] = [
		{ id: 1, judul: 'Question 1-7', isi: 'Hotels were among the earliest facilities that bound the United States together. They were both creatures of and creators of the expanding nation...', kategori: 'Reading', jumlah: 7, media: null, status: 'Aktif' },
		{ id: 6, judul: 'DIRECTIONS 1-15', isi: 'Questions 1-15 are incomplete sentences. Beneath each sentence you will see four words or phrases, marked A, B, C and D...', kategori: 'Structure', jumlah: 15, media: null, status: 'Aktif' },
		{ id: 101, judul: 'الحياة الاجتماعية', isi: 'الحياة الاجتماعية جزءًا أساسيًا من الوجود الإنساني، فهي ضرورية لتطور الفرد والمجتمع على حد سواء. ينشأ الإنسان...', kategori: 'Reading', jumlah: 10, media: null, status: 'Aktif' },
		{ id: 301, judul: 'Tarakib - Pilihan Ganda', isi: 'اختر أنسب جواب من الأجوبة الأربعة', kategori: 'Structure', jumlah: 10, media: null, status: 'Aktif' }
	];

	const extraJudul: Array<[string, string, string, number]> = [
		['Question 8-14', 'The railroad transformed the American landscape. Towns grew around stations and new markets opened...', 'Reading', 7],
		['DIRECTIONS 16-30', 'Questions 16-30 are incomplete sentences. Choose the word or phrase that best completes each sentence...', 'Structure', 15],
		['الثقافة العربية', 'الثقافة العربية غنية بالتراث والأدب والعلوم. ساهم العلماء العرب في تطور الطب والفلك والرياضيات...', 'Qiraah', 10],
		['Tarakib - Isian Singkat', 'املأ الفراغ بالكلمة المناسبة من حيث التركيب النحوي والصرفي', 'Tarakib', 12],
		['Question 15-21', 'Clouds are formed when water vapor condenses around tiny particles in the atmosphere. Their shapes reveal...', 'Reading', 7],
		['Long Passage - Environment', 'Environmental conservation has become a global priority as climate change accelerates. Governments and communities...', 'Reading', 10],
		['العبادة في الإسلام', 'العبادة في الإسلام تشمل الصلاة والصيام والزكاة والحج. وهي تنظم علاقة المسلم بربه وبمجتمعه...', 'Qiraah', 8],
		['Structure Review Batch 2', 'Review of written expression: subject-verb agreement, parallel structure, and correct word forms...', 'Structure', 20],
		['Question 22-28', 'The discovery of penicillin marked a turning point in modern medicine. Alexander Fleming observed...', 'Reading', 7],
		['النص الوصفي', 'يصف هذا النص مدينة تاريخية تقع على ضفاف النهر. تشتهر بأسواقها القديمة ومساجدها العريقة...', 'Qiraah', 10],
		['Tarakib - Tashrif Practice', 'صرف الأفعال التالية مع الضمائر المختلفة في الماضي والمضارع والأمر', 'Tarakib', 15],
		['Passage Audio Transcript 1', 'Transcript of listening audio: conversation between two students about campus facilities...', 'Listening', 5],
		['نص الاستماع الأول', 'نص المحادثة بين الطالب والأستاذ حول التسجيل في الدورات اللغوية...', 'Istima', 5],
		['Question 29-35', 'Whales are among the largest creatures on Earth. Despite their size, they feed primarily on tiny organisms...', 'Reading', 7],
		['Structure - Error Analysis', 'Identify the underlined word or phrase that must be changed for the sentence to be correct...', 'Structure', 10],
		['القراءة السريعة 1', 'تدريب على القراءة السريعة وفهم الأفكار الرئيسية في وقت محدود...', 'Qiraah', 6],
		['Tarakib - Jumlah Ismiyah', 'تدريبات على الجملة الاسمية وأركانها: المبتدأ والخبر وأنواعهما...', 'Tarakib', 10],
		['Question 36-42', 'Photography was invented in the 19th century and quickly changed how people document history...', 'Reading', 7],
		['Passage Pendukung TOEFL', 'Kumpulan teks latihan reading untuk persiapan TOEFL prediction dengan tingkat kesulitan sedang...', 'Reading', 12],
		['DIRECTIONS Batch 3', 'Written expression questions focusing on articles, prepositions, and conjunctions...', 'Structure', 15],
		['قصة قصيرة - الأمانة', 'قصة قصيرة عن قيمة الأمانة في حياة المسلم اليومية ودورها في بناء الثقة...', 'Qiraah', 8],
		['Tarakib - Huruf Jarr', 'تدريبات على حروف الجر ومعانيها ومواضع استخدامها في الجمل العربية...', 'Tarakib', 10],
		['Long Passage - Technology', 'Artificial intelligence is reshaping education. Adaptive learning platforms personalize materials...', 'Reading', 10],
		['Question 43-50', 'The Great Depression affected millions of families. Economic recovery required coordinated policies...', 'Reading', 8],
		['Nash Istima 2', 'حوار بين مسافر وموظف الاستقبال في الفندق حول الحجز والخدمات...', 'Istima', 6],
		['Audio Script - Campus Life', 'Monologue about campus facilities, library hours, and student activities...', 'Listening', 8],
		['Tarakib - Dhamir', 'تدريبات على الضمائر المنفصلة والمتصلة في محل الرفع والنصب والجر...', 'Tarakib', 10],
		['Reading Set - Health', 'Sleep plays a crucial role in memory consolidation and physical recovery. Recent studies show...', 'Reading', 9],
		['Structure - Clause Review', 'Adjective clauses, adverb clauses, and noun clauses with reduction patterns...', 'Structure', 12],
		['نص القراءة المطولة', 'نص مطول عن الحضارة الإسلامية وإسهاماتها في العلوم والفنون والعمارة...', 'Qiraah', 12]
	];

	const allData: Passage[] = [
		...base,
		...extraJudul.map(([judul, isi, kategori, jumlah], i) => ({
			id: 400 + i,
			judul,
			isi,
			kategori,
			jumlah,
			media: null as string | null,
			status: (i % 9 === 5 ? 'Nonaktif' : 'Aktif') as Passage['status']
		}))
	];

	let fKategori = $state('Semua Kategori');
	let fStatus = $state('Semua');
	let fCari = $state('');

	let sKategori = $state('Semua Kategori');
	let sStatus = $state('Semua');
	let sCari = $state('');

	let page = $state(1);
	const perPage = 10;

	let filtered = $derived(
		allData.filter((p) => {
			if (sKategori !== 'Semua Kategori' && p.kategori !== sKategori) return false;
			if (sStatus !== 'Semua' && p.status !== sStatus) return false;
			const q = sCari.trim().toLowerCase();
			if (q && !(p.judul.toLowerCase().includes(q) || p.isi.toLowerCase().includes(q))) return false;
			return true;
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
		let start = Math.max(1, Math.min(cur - 5, total - 9));
		let end = Math.min(total, start + 9);
		if (end - start < 9) start = Math.max(1, end - 9);
		return Array.from({ length: end - start + 1 }, (_, i) => start + i);
	});

	function applyFilter() {
		sKategori = fKategori;
		sStatus = fStatus;
		sCari = fCari;
		page = 1;
	}
	function resetFilter() {
		fKategori = 'Semua Kategori';
		fStatus = 'Semua';
		fCari = '';
		applyFilter();
	}

	function excerpt(t: string) {
		return t.length > 110 ? t.slice(0, 110) + '...' : t;
	}

	function hapus(id: number) {
		if (!confirm('Hapus passage ini?')) return;
		const idx = allData.findIndex((p) => p.id === id);
		if (idx >= 0) allData.splice(idx, 1);
	}
</script>

<svelte:head>
	<title>Manajemen Passage — P2BA UNIA</title>
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
			<a href="/admin/bank-soal" class="nav-item">
				<i class="fa-solid fa-book-open"></i><span>Bank Soal</span>
			</a>
			<a href="/admin/passage" class="nav-item active">
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
				<h1>Manajemen Passage</h1>
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
				<p>Kelola teks bacaan/cerita untuk soal berkelompok</p>
				<a href="/admin/passage/tambah" class="btn-primary">
					<i class="fa-solid fa-plus"></i> Tambah Passage
				</a>
			</div>

			<section class="card filter-card">
				<div class="filter-grid">
					<label><span>Kategori</span>
						<select bind:value={fKategori}>
							<option>Semua Kategori</option>
							<option>Listening</option><option>Istima</option><option>Structure</option>
							<option>Tarakib</option><option>Reading</option><option>Qiraah</option>
						</select>
					</label>
					<label><span>Status</span>
						<select bind:value={fStatus}>
							<option>Semua</option><option>Aktif</option><option>Nonaktif</option>
						</select>
					</label>
					<label class="grow"><span>Cari</span>
						<input type="text" placeholder="Cari judul/isi..." bind:value={fCari} onkeydown={(e) => e.key === 'Enter' && applyFilter()} />
					</label>
					<div class="filter-btns">
						<button class="btn-primary" onclick={applyFilter}><i class="fa-solid fa-filter"></i> Filter</button>
						<button class="btn-gray" onclick={resetFilter}>Reset</button>
					</div>
				</div>
			</section>

			<section class="card table-card">
				<div class="table-responsive">
					<table>
						<thead>
							<tr>
								<th style="width:64px">ID</th>
								<th>PASSAGE</th>
								<th class="center">KATEGORI</th>
								<th class="center">SOAL</th>
								<th class="center">MEDIA</th>
								<th class="center">STATUS</th>
								<th class="right">AKSI</th>
							</tr>
						</thead>
						<tbody>
							{#each rows as p (p.id)}
								<tr>
									<td class="id">#{p.id}</td>
									<td class="passage">
										<strong>{p.judul}</strong>
										<p>{excerpt(p.isi)}</p>
									</td>
									<td class="center"><span class="pill kat">{p.kategori}</span></td>
									<td class="center"><span class="pill blue">{p.jumlah}</span></td>
									<td class="center media">{p.media ?? '–'}</td>
									<td class="center">
										{#if p.status === 'Aktif'}
											<span class="pill green"><span class="dot"></span> Aktif</span>
										{:else}
											<span class="pill gray"><span class="dot gray"></span> Nonaktif</span>
										{/if}
									</td>
									<td class="right actions">
										<button title="Lihat" aria-label="Lihat"><i class="fa-regular fa-eye"></i></button>
										<button title="Edit" aria-label="Edit"><i class="fa-regular fa-pen-to-square"></i></button>
										<button title="Hapus" aria-label="Hapus" onclick={() => hapus(p.id)}>
											<i class="fa-regular fa-trash-can"></i>
										</button>
									</td>
								</tr>
							{/each}
							{#if rows.length === 0}
								<tr><td colspan="7" class="empty">Tidak ada passage yang cocok dengan filter.</td></tr>
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
	.toolbar { display: flex; justify-content: space-between; align-items: center; gap: 12px; }
	.toolbar p { font-size: 13.5px; color: #475569; }
	.btn-primary { background: var(--primary); color: #fff; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 700; cursor: pointer; display: inline-flex; gap: 8px; align-items: center; box-shadow: 0 4px 12px rgba(59,130,246,0.3); white-space: nowrap; }
	.btn-primary:hover { background: var(--primary-dark); }
	.btn-gray { background: #eef2f7; color: #334155; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 600; cursor: pointer; }

	.card { background: var(--bg-card); border-radius: var(--radius); box-shadow: var(--shadow-sm); }
	.filter-card { padding: 18px 20px; }
	.filter-grid { display: grid; grid-template-columns: 1fr 1fr 1.6fr auto; gap: 12px; align-items: end; }
	.filter-grid label span { font-size: 12.5px; font-weight: 700; display: block; margin-bottom: 7px; color: #1e293b; }
	.filter-grid input, .filter-grid select { width: 100%; padding: 10px 14px; border-radius: 10px; border: 1px solid var(--input-border); font-size: 13px; outline: none; background: #fff; color: var(--text-dark); }
	.filter-grid input::placeholder { color: #9aa5b5; }
	.filter-grid input:focus, .filter-grid select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px var(--primary-light); }
	.filter-btns { display: flex; gap: 8px; }

	.table-card { padding: 0; overflow: hidden; }
	.table-responsive { overflow-x: auto; }
	table { width: 100%; border-collapse: collapse; font-size: 13px; }
	th { text-align: left; padding: 12px 16px; color: #64748b; font-size: 11px; letter-spacing: 0.5px; border-bottom: 1px solid var(--border-color); background: #fbfcfe; white-space: nowrap; }
	td { padding: 14px 16px; border-bottom: 1px solid var(--border-color); vertical-align: middle; background: #fff; }
	tbody tr:last-child td { border-bottom: none; }
	tbody tr:hover td { background: #f8fafc; }
	.center { text-align: center; }
	.right { text-align: right; }
	.id { color: #64748b; white-space: nowrap; }
	.passage strong { font-size: 13.5px; font-weight: 700; display: block; margin-bottom: 4px; }
	.passage p { font-size: 12.5px; color: #475569; line-height: 1.5; }
	.media { color: #94a3b8; }
	.pill { display: inline-flex; align-items: center; gap: 5px; font-size: 11.5px; font-weight: 700; padding: 4px 11px; border-radius: 99px; white-space: nowrap; }
	.pill.kat { background: #f1f5f9; color: #475569; }
	.pill.blue { background: var(--primary-light); color: #1d4ed8; }
	.pill.green { background: #dcfce7; color: #15803d; }
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

	@media (max-width: 1024px) { .filter-grid { grid-template-columns: 1fr 1fr; } }
	@media (max-width: 768px) {
		.page { flex-direction: column; }
		.sidebar { width: 100%; position: static; height: auto; }
		.filter-grid { grid-template-columns: 1fr; }
		.toolbar { flex-direction: column; align-items: stretch; }
		.pager-row { flex-direction: column; align-items: stretch; }
		.pager { overflow-x: auto; }
	}
</style>
