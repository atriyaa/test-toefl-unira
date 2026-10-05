<script lang="ts">
	let email = $state('');
	let password = $state('');
	let showPassword = $state(false);
	let remember = $state(true);

	function handleLogin(e: SubmitEvent) {
		e.preventDefault();
		window.location.href = '/admin/dashboard';
	}
</script>

<svelte:head>
	<title>Login — SRMS</title>
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

<div class="login-page">
	<!-- LEFT : branding panel -->
	<section class="brand-panel">
		<div class="brand-glow g1"></div>
		<div class="brand-glow g2"></div>

		<div class="brand-top">
			<div class="brand-logo">
				<i class="fa-solid fa-graduation-cap"></i>
				<div>
					<h2>SRMS</h2>
					<p>Student Result Management System</p>
				</div>
			</div>
		</div>

		<div class="brand-main">
			<div class="brand-illustration">
				<i class="fa-solid fa-book-bookmark"></i>
			</div>
			<h1>Stay updated with your academic journey.</h1>
			<p class="brand-sub">
				Pantau CGPA, hasil ujian, kehadiran, dan performa dalam satu dashboard yang modern.
			</p>

			<ul class="brand-features">
				<li><i class="fa-solid fa-circle-check"></i> Recent Results & Grade otomatis</li>
				<li><i class="fa-solid fa-circle-check"></i> Academic Calendar & Notice Board</li>
				<li><i class="fa-solid fa-circle-check"></i> Performance Overview 84% real-time</li>
			</ul>

			<div class="brand-stats">
				<div class="b-stat">
					<strong>8.45</strong>
					<span>CGPA</span>
				</div>
				<div class="b-stat">
					<strong>87%</strong>
					<span>Attendance</span>
				</div>
				<div class="b-stat">
					<strong>12</strong>
					<span>Rank / 120</span>
				</div>
			</div>
		</div>

		<div class="brand-foot">
			<span>&copy; 2024 Student Result Management System.</span>
		</div>
	</section>

	<!-- RIGHT : form panel -->
	<section class="form-panel">
		<div class="form-card">
			<div class="mobile-logo">
				<i class="fa-solid fa-graduation-cap"></i>
				<span>SRMS</span>
			</div>

			<h2>Welcome back 👋</h2>
			<p class="muted">Masuk untuk melanjutkan ke Dashboard.</p>

			<form onsubmit={handleLogin}>
				<label class="field">
					<span>Email Address</span>
					<div class="input-wrap">
						<i class="fa-regular fa-envelope"></i>
						<input
							type="email"
							placeholder="nama@kampus.ac.id"
							bind:value={email}
							required
							autocomplete="username"
						/>
					</div>
				</label>

				<label class="field">
					<span>Password</span>
					<div class="input-wrap">
						<i class="fa-solid fa-lock"></i>
						<input
							type={showPassword ? 'text' : 'password'}
							placeholder="••••••••"
							bind:value={password}
							required
							autocomplete="current-password"
						/>
						<button
							type="button"
							class="eye"
							onclick={() => (showPassword = !showPassword)}
							aria-label="Tampilkan password"
						>
							<i class={showPassword ? 'fa-regular fa-eye-slash' : 'fa-regular fa-eye'}></i>
						</button>
					</div>
				</label>

				<div class="form-row">
					<label class="remember">
						<input type="checkbox" bind:checked={remember} />
						<span class="box"><i class="fa-solid fa-check"></i></span>
						Ingat saya
					</label>
					<a href="/login" class="forgot">Lupa password?</a>
				</div>

				<button type="submit" class="btn-primary">
					Login to Dashboard <i class="fa-solid fa-arrow-right"></i>
				</button>
			</form>

			<div class="divider"><span>atau</span></div>

			<div class="quick-grid">
				<a href="/admin/dashboard" class="quick-btn">
					<i class="fa-solid fa-file-invoice"></i><span>My Results</span>
				</a>
				<a href="/admin/dashboard" class="quick-btn">
					<i class="fa-solid fa-calendar-days"></i><span>Timetable</span>
				</a>
				<a href="/admin/dashboard" class="quick-btn">
					<i class="fa-solid fa-circle-down"></i><span>Downloads</span>
				</a>
			</div>

			<p class="signup">Belum punya akun? <a href="/login">Hubungi admin akademik</a></p>
		</div>
	</section>
</div>

<style>
	:root {
		--bg-main: #f4f7fe;
		--bg-card: #ffffff;
		--primary: #3b82f6;
		--primary-light: #eff6ff;
		--primary-gradient: linear-gradient(135deg, #2563eb, #3b82f6);
		--text-dark: #1e293b;
		--text-muted: #64748b;
		--border-color: #f1f5f9;
		--shadow-sm: 0 4px 6px -1px rgba(0, 0, 0, 0.03), 0 2px 4px -1px rgba(0, 0, 0, 0.02);
		--radius: 14px;
	}
	:global(*) {
		box-sizing: border-box;
		margin: 0;
		padding: 0;
		font-family: 'Plus Jakarta Sans', sans-serif;
	}
	.login-page {
		display: grid;
		grid-template-columns: 1fr 1fr;
		min-height: 100vh;
		background: var(--bg-main);
		color: var(--text-dark);
	}

	/* ===== LEFT ===== */
	.brand-panel {
		position: relative;
		overflow: hidden;
		background: var(--primary-gradient);
		color: #fff;
		padding: 32px 44px;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		border-top-right-radius: 24px;
		border-bottom-right-radius: 24px;
	}
	.brand-glow {
		position: absolute;
		border-radius: 50%;
		background: rgba(255, 255, 255, 0.12);
	}
	.brand-glow.g1 {
		width: 340px;
		height: 340px;
		top: -100px;
		right: -100px;
	}
	.brand-glow.g2 {
		width: 220px;
		height: 220px;
		bottom: 60px;
		left: -70px;
		background: rgba(255, 255, 255, 0.08);
	}
	.brand-logo {
		display: flex;
		align-items: center;
		gap: 12px;
		position: relative;
		z-index: 1;
	}
	.brand-logo i {
		font-size: 30px;
		background: rgba(255, 255, 255, 0.18);
		width: 52px;
		height: 52px;
		display: flex;
		align-items: center;
		justify-content: center;
		border-radius: 14px;
	}
	.brand-logo h2 {
		font-size: 20px;
		letter-spacing: 1px;
		font-weight: 800;
	}
	.brand-logo p {
		font-size: 11px;
		opacity: 0.85;
	}
	.brand-main {
		position: relative;
		z-index: 1;
		max-width: 440px;
	}
	.brand-illustration {
		font-size: 52px;
		background: rgba(255, 255, 255, 0.15);
		width: 88px;
		height: 88px;
		border-radius: 20px;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-bottom: 20px;
	}
	.brand-main h1 {
		font-size: 30px;
		line-height: 1.25;
		font-weight: 800;
		margin-bottom: 10px;
	}
	.brand-sub {
		font-size: 14px;
		opacity: 0.9;
		margin-bottom: 20px;
	}
	.brand-features {
		list-style: none;
		display: flex;
		flex-direction: column;
		gap: 10px;
		font-size: 13px;
		margin-bottom: 24px;
	}
	.brand-features i {
		margin-right: 8px;
	}
	.brand-stats {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 12px;
	}
	.b-stat {
		background: rgba(255, 255, 255, 0.15);
		border: 1px solid rgba(255, 255, 255, 0.2);
		border-radius: var(--radius);
		padding: 14px;
		text-align: center;
		backdrop-filter: blur(6px);
	}
	.b-stat strong {
		display: block;
		font-size: 20px;
		font-weight: 800;
	}
	.b-stat span {
		font-size: 11px;
		opacity: 0.9;
	}
	.brand-foot {
		font-size: 12px;
		opacity: 0.8;
		position: relative;
		z-index: 1;
	}

	/* ===== RIGHT ===== */
	.form-panel {
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 40px 24px;
	}
	.form-card {
		background: var(--bg-card);
		border-radius: var(--radius);
		box-shadow: var(--shadow-sm);
		padding: 36px;
		width: 100%;
		max-width: 430px;
	}
	.mobile-logo {
		display: none;
		align-items: center;
		gap: 8px;
		font-weight: 800;
		color: var(--primary);
		margin-bottom: 16px;
	}
	.mobile-logo i {
		font-size: 24px;
	}
	.form-card h2 {
		font-size: 24px;
		font-weight: 800;
		margin-bottom: 4px;
	}
	.muted {
		font-size: 13px;
		color: var(--text-muted);
		margin-bottom: 24px;
	}
	.field {
		display: block;
		margin-bottom: 16px;
	}
	.field > span {
		font-size: 12px;
		font-weight: 600;
		display: block;
		margin-bottom: 8px;
	}
	.input-wrap {
		position: relative;
		display: flex;
		align-items: center;
	}
	.input-wrap > i {
		position: absolute;
		left: 14px;
		color: var(--text-muted);
		font-size: 14px;
	}
	.input-wrap input {
		width: 100%;
		padding: 12px 40px 12px 40px;
		border-radius: 12px;
		border: 1px solid #e2e8f0;
		background: #f8fafc;
		font-size: 14px;
		outline: none;
		transition: all 0.2s;
		color: var(--text-dark);
	}
	.input-wrap input:focus {
		border-color: var(--primary);
		background: #fff;
		box-shadow: 0 0 0 4px var(--primary-light);
	}
	.eye {
		position: absolute;
		right: 8px;
		background: none;
		border: none;
		color: var(--text-muted);
		cursor: pointer;
		width: 32px;
		height: 32px;
		border-radius: 8px;
	}
	.eye:hover {
		background: var(--primary-light);
		color: var(--primary);
	}
	.form-row {
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 12px;
		margin: 4px 0 20px;
	}
	.remember {
		display: flex;
		align-items: center;
		gap: 8px;
		cursor: pointer;
		color: var(--text-muted);
		font-weight: 500;
	}
	.remember input {
		display: none;
	}
	.remember .box {
		width: 18px;
		height: 18px;
		border-radius: 6px;
		border: 1px solid #cbd5e1;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 10px;
		color: #fff;
		background: #fff;
	}
	.remember input:checked + .box {
		background: var(--primary);
		border-color: var(--primary);
	}
	.forgot {
		color: var(--primary);
		font-weight: 600;
		text-decoration: none;
	}
	.btn-primary {
		width: 100%;
		border: none;
		cursor: pointer;
		background: var(--primary);
		color: #fff;
		font-weight: 700;
		font-size: 14px;
		padding: 13px;
		border-radius: 12px;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 10px;
		box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
		transition: all 0.2s;
	}
	.btn-primary:hover {
		transform: translateY(-1px);
		box-shadow: 0 8px 20px rgba(59, 130, 246, 0.35);
	}
	.divider {
		display: flex;
		align-items: center;
		gap: 12px;
		color: var(--text-muted);
		font-size: 11px;
		margin: 22px 0 14px;
	}
	.divider::before,
	.divider::after {
		content: '';
		flex: 1;
		height: 1px;
		background: var(--border-color);
	}
	.quick-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 10px;
	}
	.quick-btn {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 6px;
		padding: 12px 6px;
		background: #f8fafc;
		border-radius: 12px;
		text-decoration: none;
		color: var(--text-dark);
		transition: all 0.2s;
	}
	.quick-btn:hover {
		background: var(--primary-light);
		transform: translateY(-2px);
	}
	.quick-btn i {
		color: var(--primary);
		font-size: 18px;
	}
	.quick-btn span {
		font-size: 11px;
		font-weight: 600;
	}
	.signup {
		margin-top: 20px;
		font-size: 12px;
		color: var(--text-muted);
		text-align: center;
	}
	.signup a {
		color: var(--primary);
		font-weight: 700;
		text-decoration: none;
	}

	@media (max-width: 900px) {
		.login-page {
			grid-template-columns: 1fr;
		}
		.brand-panel {
			border-radius: 0 0 24px 24px;
			padding: 28px 24px;
			gap: 24px;
		}
		.brand-main h1 {
			font-size: 24px;
		}
		.brand-foot {
			display: none;
		}
		.mobile-logo {
			display: flex;
		}
	}
</style>
