<!-- src/routes/+layout.svelte (하단 바 축소 및 스크롤 방지 최종본) -->

<script>
	import { goto } from '$app/navigation';
	function goToHome() { goto('/'); }
	function goBack() { history.back(); }
</script>

<div class="page-wrapper">
	<div class="content-area">
		<slot />
	</div>

	<footer class="kiosk-footer">
		<button class="nav-item" on:click={goToHome}>
			<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>
			<span>처음으로</span>
		</button>
		<button class="nav-item">
			<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m18 13-6-6-6 6"/><path d="m18 7-6-6-6 6"/></svg>
			<span>직원호출</span>
		</button>
		<button class="nav-item" on:click={goBack}>
			<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M19 12H5"/><polyline points="12 19 5 12 12 5"/></svg>
			<span>뒤로가기</span>
		</button>
	</footer>
</div>

<style>
	/* --- ▼▼▼ 페이지 전체 스크롤 방지 ▼▼▼ --- */
	:global(body, html) {
		margin: 0; padding: 0; height: 100vh;
		/* 핵심: 브라우저 레벨의 모든 스크롤을 원천적으로 차단합니다. */
		overflow: hidden; 
		font-family: 'Pretendard', -apple-system, sans-serif;
		background-color: #f8f9fa; 
	}

	.page-wrapper {
		display: flex; flex-direction: column; height: 100%;
	}
	.content-area {
		flex: 1; 
		/* 내용이 길어질 경우, 이 영역 '안에서만' 스크롤이 생깁니다. */
		overflow-y: auto; 
		position: relative;
	}

	/* --- ▼▼▼ 하단 바 크기 축소 ▼▼▼ --- */
	.kiosk-footer {
		flex-shrink: 0;
		display: flex; justify-content: space-around; align-items: center; 
		background-color: #ffffff; /* 하단 바 배경색을 흰색으로 명확히 구분 */
		border-top: 1px solid #e9ecef; 
		padding: 0.5rem 0; /* 상하 여백을 줄여 높이를 줄임 */
	}
	.nav-item { 
		background: none; border: none; cursor: pointer; 
		display: flex; flex-direction: column; align-items: center; 
		gap: 0.2rem; /* 아이콘과 텍스트 사이 간격을 줄임 */
		color: #868e96; 
		font-size: 0.75rem; /* 폰트 크기를 줄임 */
		padding: 0.25rem; 
		transition: color 0.15s; 
	}
	.nav-item:hover { color: #212529; }
	.nav-item svg { 
		width: 24px; height: 24px; /* 아이콘 크기를 줄임 */
	}
</style>