<!-- src/routes/+layout.svelte -->

<div class="kiosk-frame">
	<!-- <slot />은 SvelteKit에서 "페이지 내용이 여기에 들어온다"는 의미의 특별한 태그입니다. -->
	<slot />
</div>

<style>
	/* 이 스타일은 모든 페이지에 공통으로 적용됩니다. */
	:global(body, html) {
		margin: 0;
		padding: 0;
		height: 100%;
		font-family: 'Pretendard', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
		background-color: #e9ecef;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.kiosk-frame {
		width: 50vh;
		height: 95vh;
		max-width: 100vw;
		background: #f8f9fa;
		border-radius: 40px;
		box-shadow: 0 10px 50px rgba(0, 0, 0, 0.2);
		display: flex;
		flex-direction: column; /* 페이지 내용이 세로로 채워지도록 */
		overflow: hidden;
	}
</style>