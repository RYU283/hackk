<div class="kiosk-shell">
	<div class="kiosk-frame">
		<!-- <slot />: 페이지의 실제 내용(메인, 주문, 결제)이 이 자리에 렌더링됩니다. -->
		<slot />
	</div>
</div>

<style>
	/* :global() 선택자는 이 스타일을 프로젝트 전체에 적용시킵니다. */
	:global(body, html) {
		margin: 0;
		padding: 0;
		height: 100vh;
		/* 핵심: 페이지 자체의 스크롤을 원천적으로 차단합니다. */
		overflow: hidden;
		font-family: 'Pretendard', -apple-system, sans-serif;
	}

	/* 화면 전체를 덮는 어두운 배경 (실제 키오스크 주변 환경) */
	.kiosk-shell {
		width: 100vw;
		height: 100vh;
		background-color: #343a40; /* 어두운 배경색 */
		display: flex;
		justify-content: center;
		align-items: center;
	}

	/* 모든 페이지가 들어갈 고정된 프레임 (실제 키오스크 화면) */
	.kiosk-frame {
		width: 50vh; /* 1:2 비율 유지 */
		height: 95vh;
		max-width: 100vw;
		background: #f8f9fa; /* 기본 흰색 계열 배경 */
		border-radius: 40px;
		box-shadow: 0 10px 60px rgba(0, 0, 0, 0.4);
		
		/* 내부 페이지가 프레임을 꽉 채우도록 설정 */
		display: flex;
		flex-direction: column;
		overflow: hidden; /* 프레임 내부에서도 스크롤이 생기지 않도록 방지 */
	}
</style>