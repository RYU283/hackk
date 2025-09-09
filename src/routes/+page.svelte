<!-- src/routes/+page.svelte (레이아웃 적용 후) -->
<script>
	import { goto } from '$app/navigation';
	let isAiEnabled = false;
	function toggleAiFeature() { isAiEnabled = !isAiEnabled; }
	function handleSelection(type) { goto('/order'); }
	function goBack() { history.back(); }
	function goToHome() { goto('/'); }
</script>

<!-- 이제부터 페이지는 내용물만 신경쓰면 됩니다. -->
<header class="kiosk-header">
	<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 21h18v-2H3v2zM5 19h14v-2H5v2zM6 17h12v-2H6v2zM12 5a3 3 0 1 0 0-6 3 3 0 0 0 0 6z"/><path d="M12 5v12"/></svg>
	<span>SVELTE RESTAURANT</span>
</header>

<main class="kiosk-content">
	<div class="welcome-text"><h1>안녕하세요!<br>무엇을 도와드릴까요?</h1></div>
	<div class="button-grid">
		<button class="grid-button primary" on:click={() => handleSelection('매장 식사')}>
			<span class="main-text">매장 식사</span><span class="sub-text">Dine in</span>
			<div class="button-icon"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"><path d="M11 9H9V2H7v7H5V2H3v7c0 2.12 1.66 3.84 3.75 3.97V22h2.5v-9.03C11.34 12.84 13 11.12 13 9V2h-2v7zm5-3v8h2.5v8H21V2c-2.76 0-5 2.24-5 4z" /></svg></div>
		</button>
		<button class="grid-button primary" on:click={() => handleSelection('포장 주문')}>
			<span class="main-text">포장 주문</span><span class="sub-text">Takeout</span>
			<div class="button-icon"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"><path d="M18 6h-2c0-2.21-1.79-4-4-4S8 3.79 8 6H6c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h12c1.1 0 2-.9 2-2V8c0-1.1-.9-2-2-2zm-6-2c1.1 0 2 .9 2 2h-4c0-1.1.9-2 2-2zm6 16H6V8h2v2c0 .55.45 1 1 1s1-.45 1-1V8h4v2c0 .55.45 1 1 1s1-.45 1-1V8h2v12z" /></svg></div>
		</button>
		<div class="grid-button secondary full-width ai-toggle-container" on:click={toggleAiFeature} role="button" tabindex="0">
			<div><span class="main-text">AI 음성 주문</span><span class="sub-text">{#if isAiEnabled}현재 활성화됨{:else}터치하여 활성화{/if}</span></div>
			<div class="toggle-switch" class:active={isAiEnabled}><div class="toggle-handle" /></div>
		</div>
	</div>
</main>

<footer class="kiosk-footer">
	<button class="nav-item" on:click={goToHome}><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg><span>처음으로</span></button>
	<button class="nav-item"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m18 13-6-6-6 6"/><path d="m18 7-6-6-6 6"/></svg><span>직원호출</span></button>
	<button class="nav-item" on:click={goBack}><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M19 12H5"/><polyline points="12 19 5 12 12 5"/></svg><span>뒤로가기</span></button>
</footer>

<style>
	/* 스타일에서 프레임 관련 코드가 모두 사라지고, 내용물 스타일만 남았습니다. */
	header, main, footer { padding-left: 2.5rem; padding-right: 2.5rem; box-sizing: border-box; }
	.kiosk-header { display: flex; align-items: center; gap: 0.5rem; padding-top: 1.5rem; padding-bottom: 1.5rem; flex-shrink: 0; font-weight: 600; color: #495057; }
	.kiosk-content { flex: 1; display: flex; flex-direction: column; }
	.welcome-text h1 { font-size: 2.8rem; font-weight: 700; color: #212529; line-height: 1.3; margin-bottom: 2rem; }
	.button-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1.25rem; flex: 1; }
	.grid-button { position: relative; border: none; border-radius: 20px; padding: 1.5rem; display: flex; flex-direction: column; justify-content: flex-start; align-items: flex-start; text-align: left; cursor: pointer; transition: all 0.2s; overflow: hidden; }
	.grid-button:hover { transform: translateY(-6px); box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12); }
	.grid-button.primary { background-color: #1c7ed6; color: white; }
	.grid-button.secondary { background-color: #ffffff; color: #343a40; border: 1px solid #e9ecef; }
	.grid-button.full-width { grid-column: 1 / -1; }
	.main-text { font-size: 1.8rem; font-weight: 700; }
	.sub-text { font-size: 1rem; font-weight: 400; opacity: 0.8; }
	.button-icon { position: absolute; bottom: 1rem; right: 1.25rem; opacity: 0.5; }
	.button-icon svg { width: 48px; height: 48px; }
	.kiosk-footer { display: flex; justify-content: space-around; align-items: center; background-color: #ffffff; border-top: 1px solid #e9ecef; margin: 0 -2.5rem; padding: 0.5rem 2.5rem; flex-shrink: 0; }
	.nav-item { background: none; border: none; cursor: pointer; display: flex; flex-direction: column; align-items: center; gap: 0.25rem; color: #868e96; font-size: 0.8rem; padding: 0.5rem; border-radius: 8px; transition: all 0.15s; }
	.nav-item:hover { color: #212529; background-color: #f1f3f5; }
	.nav-item svg { width: 28px; height: 28px; }
	.ai-toggle-container { flex-direction: row; justify-content: space-between; align-items: center; }
	.toggle-switch { width: 64px; height: 34px; background-color: #dee2e6; border-radius: 17px; position: relative; transition: background-color 0.2s; }
	.toggle-switch.active { background-color: #1c7ed6; }
	.toggle-handle { width: 28px; height: 28px; background-color: white; border-radius: 50%; position: absolute; top: 3px; left: 3px; transition: transform 0.2s; box-shadow: 0 2px 4px rgba(0,0,0,0.2); }
	.toggle-switch.active .toggle-handle { transform: translateX(30px); }
</style>