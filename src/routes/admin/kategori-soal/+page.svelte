<script lang="ts">
	type Kategori = {
		id: number;
		nama: string;
		deskripsi: string;
		durasi: number;
		jumlah: number;
		status: 'Aktif' | 'Nonaktif';
	};

	let data = $state<Kategori[]>([
		{ id: 1, nama: 'Listening', deskripsi: 'Listening Comprehension Section', durasi: 35, jumlah: 0, status: 'Aktif' },
		{ id: 2, nama: 'Istima', deskripsi: 'Listening Comprehension (Istima)', durasi: 35, jumlah: 0, status: 'Aktif' },
		{ id: 3, nama: 'Structure', deskripsi: 'Structure and Written Expression Section', durasi: 25, jumlah: 40, status: 'Aktif' },
		{ id: 4, nama: 'Tarakib', deskripsi: 'Structure and Written Expression (Tarakib)', durasi: 25, jumlah: 40, status: 'Aktif' },
		{ id: 5, nama: 'Reading', deskripsi: 'Reading Comprehension Section', durasi: 55, jumlah: 50, status: 'Aktif' },
		{ id: 6, nama: 'Qiraah', deskripsi: 'Reading Comprehension (Qiraah)', durasi: 55, jumlah: 0, status: 'Aktif' }
	]);

	let dragId = $state<number | null>(null);

	let showModal = $state(false);
	let editing = $state<Kategori | null>(null);
	let fNama = $state('');
	let fDeskripsi = $state('');
	let fDurasi = $state(30);
	let fStatus = $state<'Aktif' | 'Nonaktif'>('Aktif');

	let deleting = $state<Kategori | null>(null);

	function openTambah() {
		editing = null;
		fNama = '';
		fDeskripsi = '';
		fDurasi = 30;
		fStatus = 'Aktif';
		showModal = true;
	}

	function openEdit(k: Kategori) {
		editing = k;
		fNama = k.nama;
		fDeskripsi = k.deskripsi;
		fDurasi = k.durasi;
		fStatus = k.status;
		showModal = true;
	}

	function simpan() {
		if (!fNama.trim()) return;
		if (editing) {
			editing.nama = fNama.trim();
			editing.deskripsi = fDeskripsi.trim();
			editing.durasi = fDurasi;
			editing.status = fStatus;
		} else {
			const id = Math.max(0, ...data.map((d) => d.id)) + 1;
			data = [...data, { id, nama: fNama.trim(), deskripsi: fDeskripsi.trim(), durasi: fDurasi, jumlah: 0, status: fStatus }];
		}
		showModal = false;
	}

	function hapus() {
		if (!deleting) return;
		data = data.filter((d) => d.id !== deleting!.id);
		deleting = null;
	}

	function onDragStart(id: number) {
		dragId = id;
	}
	function onDragOver(e: DragEvent) {
		e.preventDefault();
	}
	function onDrop(targetId: number) {
		if (dragId === null || dragId === targetId) return;
		const from = data.findIndex((d) => d.id === dragId);
		const to = data.findIndex((d) => d.id === targetId);
		if (from < 0 || to < 0) return;
		const copy = [...data];
		const [moved] = copy.splice(from, 1);
		copy.splice(to, 0, moved);
		data = copy;
		dragId = null;
	}
</script>

<svelte:head>
	<title>Manajemen Kategori Soal — P2BA UNIA</title>
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
				<h1>Manajemen Kategori Soal</h1>
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
				<p>Kelola kategori soal TOEFL dengan durasi waktu masing-masing</p>
				<a href="/admin/kategori-soal/tambah" class="btn-primary">
					<i class="fa-solid fa-plus"></i> Tambah Kategori
				</a>
			</div>

			<div class="infobox">
				<i class="fa-solid fa-circle-info"></i>
				<div>
					<strong>Drag & Drop untuk mengatur urutan</strong>
					<p>Urutan kategori akan menentukan urutan tampilan soal pada saat ujian.</p>
				</div>
			</div>

			<section class="card table-card">
				<div class="table-responsive">
					<table>
						<thead>
							<tr>
								<th class="center" style="width:56px"></th>
								<th class="center" style="width:80px">URUTAN</th>
								<th>NAMA KATEGORI</th>
								<th>DESKRIPSI</th>
								<th class="center">DURASI (MENIT)</th>
								<th class="center">JUMLAH SOAL</th>
								<th class="center">STATUS</th>
								<th class="right">AKSI</th>
							</tr>
						</thead>
						<tbody>
							{#each data as k, i (k.id)}
								<tr
									draggable="true"
									ondragstart={() => onDragStart(k.id)}
									ondragover={onDragOver}
									ondrop={() => onDrop(k.id)}
									class:dragging={dragId === k.id}
								>
									<td class="center grip" title="Seret untuk mengubah urutan">
										<i class="fa-solid fa-grip-vertical"></i>
									</td>
									<td class="center"><span class="urutan">{i + 1}</span></td>
									<td class="nama">{k.nama}</td>
									<td class="desc">{k.deskripsi}</td>
									<td class="center">
										<span class="pill blue"><i class="fa-regular fa-clock"></i> {k.durasi}</span>
									</td>
									<td class="center">
										<span class="pill purple">{k.jumlah} soal</span>
									</td>
									<td class="center">
										{#if k.status === 'Aktif'}
											<span class="pill green"><span class="dot"></span> Aktif</span>
										{:else}
											<span class="pill gray"><span class="dot gray"></span> Nonaktif</span>
										{/if}
									</td>
									<td class="right actions">
										<button title="Edit" aria-label="Edit" onclick={() => openEdit(k)}>
											<i class="fa-regular fa-pen-to-square"></i>
										</button>
										<button title="Hapus" aria-label="Hapus" onclick={() => (deleting = k)}>
											<i class="fa-regular fa-trash-can"></i>
										</button>
									</td>
								</tr>
							{/each}
						</tbody>
					</table>
				</div>
			</section>
		</div>
	</main>
</div>

{#if showModal}
	<div class="overlay" role="dialog" aria-modal="true">
		<div class="modal">
			<h3>{editing ? 'Edit Kategori' : 'Tambah Kategori'}</h3>
			<p class="msub">Lengkapi data kategori soal di bawah ini.</p>
			<label class="field"><span>Nama Kategori</span>
				<input type="text" placeholder="Contoh: Listening" bind:value={fNama} />
			</label>
			<label class="field"><span>Deskripsi</span>
				<input type="text" placeholder="Contoh: Listening Comprehension Section" bind:value={fDeskripsi} />
			</label>
			<div class="grid2">
				<label class="field"><span>Durasi (menit)</span>
					<input type="number" min="1" bind:value={fDurasi} />
				</label>
				<label class="field"><span>Status</span>
					<select bind:value={fStatus}>
						<option>Aktif</option>
						<option>Nonaktif</option>
					</select>
				</label>
			</div>
			<div class="mfoot">
				<button class="btn-gray" onclick={() => (showModal = false)}>Batal</button>
				<button class="btn-primary" onclick={simpan}>Simpan</button>
			</div>
		</div>
	</div>
{/if}

{#if deleting}
	<div class="overlay" role="dialog" aria-modal="true">
		<div class="modal small">
			<h3>Hapus Kategori?</h3>
			<p class="msub">Kategori <strong>{deleting.nama}</strong> akan dihapus permanen.</p>
			<div class="mfoot">
				<button class="btn-gray" onclick={() => (deleting = null)}>Batal</button>
				<button class="btn-danger" onclick={hapus}>Hapus</button>
			</div>
		</div>
	</div>
{/if}

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

	.container { padding: 20px 24px 28px; display: flex; flex-direction: column; gap: 14px; }
	.toolbar { display: flex; justify-content: space-between; align-items: center; gap: 12px; }
	.toolbar p { font-size: 13.5px; color: #475569; }
	.btn-primary { background: var(--primary); color: #fff; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 700; cursor: pointer; display: inline-flex; gap: 8px; align-items: center; box-shadow: 0 4px 12px rgba(59,130,246,0.3); white-space: nowrap; }
	.btn-primary:hover { background: var(--primary-dark); }
	.btn-gray { background: #eef2f7; color: #334155; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 600; cursor: pointer; }
	.btn-danger { background: #ef4444; color: #fff; border: none; border-radius: 10px; padding: 10px 18px; font-size: 13px; font-weight: 700; cursor: pointer; }

	.infobox { display: flex; gap: 12px; background: var(--primary-light); border: 1px solid #bfdbfe; border-radius: 12px; padding: 14px 16px; color: #1d4ed8; font-size: 13px; }
	.infobox i { font-size: 18px; margin-top: 2px; color: var(--primary); }
	.infobox strong { font-size: 13.5px; display: block; margin-bottom: 2px; color: #1e40af; }
	.infobox p { font-size: 12.5px; color: #3b82f6; }

	.card { background: var(--bg-card); border-radius: var(--radius); box-shadow: var(--shadow-sm); }
	.table-card { padding: 0; overflow: hidden; }
	.table-responsive { overflow-x: auto; }
	table { width: 100%; border-collapse: collapse; font-size: 13px; }
	th { text-align: left; padding: 12px 16px; color: #64748b; font-size: 11px; letter-spacing: 0.5px; border-bottom: 1px solid var(--border-color); background: #f8fafc; white-space: nowrap; }
	td { padding: 14px 16px; border-bottom: 1px solid var(--border-color); vertical-align: middle; background: #fff; }
	tbody tr:last-child td { border-bottom: none; }
	tbody tr:hover td { background: #f8fafc; }
	tbody tr.dragging td { background: var(--primary-light); }
	.center { text-align: center; }
	.right { text-align: right; }
	.grip { color: #cbd5e1; cursor: grab; }
	tbody tr { cursor: grab; }
	tbody tr:active { cursor: grabbing; }

	.urutan { display: inline-flex; width: 32px; height: 32px; border-radius: 50%; background: #f1f5f9; color: #475569; font-weight: 700; font-size: 13px; align-items: center; justify-content: center; }
	.nama { font-weight: 700; font-size: 13.5px; white-space: nowrap; }
	.desc { color: #475569; font-size: 12.5px; }

	.pill { display: inline-flex; align-items: center; gap: 5px; font-size: 12px; font-weight: 700; padding: 5px 12px; border-radius: 99px; white-space: nowrap; }
	.pill.blue { background: var(--primary-light); color: #1d4ed8; }
	.pill.purple { background: #f5f3ff; color: #7c3aed; }
	.pill.green { background: #dcfce7; color: #15803d; }
	.pill.gray { background: #f1f5f9; color: #64748b; }
	.dot { width: 7px; height: 7px; border-radius: 50%; background: #22c55e; display: inline-block; }
	.dot.gray { background: #94a3b8; }

	.actions { white-space: nowrap; }
	.actions button { background: none; border: none; color: #64748b; cursor: pointer; padding: 7px; border-radius: 8px; font-size: 14px; }
	.actions button:hover { background: var(--primary-light); color: var(--primary); }

	.overlay { position: fixed; inset: 0; background: rgba(15,23,42,0.45); display: flex; align-items: center; justify-content: center; z-index: 50; padding: 16px; }
	.modal { background: #fff; border-radius: 16px; padding: 24px; width: 480px; max-width: 100%; box-shadow: 0 20px 40px rgba(0,0,0,0.15); }
	.modal.small { width: 380px; }
	.modal h3 { font-size: 17px; font-weight: 800; }
	.msub { font-size: 12.5px; color: var(--text-muted); margin: 4px 0 16px; }
	.field { display: block; margin-bottom: 12px; }
	.field > span { font-size: 12.5px; font-weight: 700; display: block; margin-bottom: 6px; }
	.field input, .field select { width: 100%; padding: 10px 14px; border-radius: 10px; border: 1px solid #e2e8f0; font-size: 13px; outline: none; background: #f8fafc; color: var(--text-dark); }
	.field input:focus, .field select:focus { border-color: var(--primary); background: #fff; box-shadow: 0 0 0 3px var(--primary-light); }
	.grid2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
	.mfoot { display: flex; justify-content: flex-end; gap: 8px; margin-top: 16px; }

	@media (max-width: 768px) {
		.page { flex-direction: column; }
		.sidebar { width: 100%; position: static; height: auto; }
		.toolbar { flex-direction: column; align-items: stretch; }
		.grid2 { grid-template-columns: 1fr; }
	}
</style>
