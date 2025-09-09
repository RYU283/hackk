<script>
	import { goto } from '$app/navigation';
	import { cart as cartStore, cartTotal as cartTotalStore } from '$lib/cartStore.js';
	const categories = [ { id: 'korea', name: '한식' }, { id: 'japan', name: '일식' }, { id: 'china', name: '중식' }, { id: 'western', name: '양식' } ];
	const menuItems = [
		// --- 한식 (16개) ---
		{ id: 1, categoryId: 'korea', name: '비빔밥', price: 9000, image: '🍚' },
		{ id: 2, categoryId: 'korea', name: '김치찌개', price: 8500, image: '🥘' },
		{ id: 3, categoryId: 'korea', name: '불고기 덮밥', price: 10000, image: '🥩' },
		{ id: 4, categoryId: 'korea', name: '해물파전', price: 12000, image: '🥞' },
		{ id: 5, categoryId: 'korea', name: '된장찌개', price: 8500, image: '🍲' },
		{ id: 6, categoryId: 'korea', name: '제육볶음', price: 9500, image: '🍖' },
		{ id: 7, categoryId: 'korea', name: '갈비탕', price: 13000, image: '🥣' },
		{ id: 8, categoryId: 'korea', name: '순두부찌개', price: 9000, image: '🌶️' },
		{ id: 9, categoryId: 'korea', name: '잡채', price: 11000, image: '🍜' },
		{ id: 10, categoryId: 'korea', name: '떡볶이', price: 6000, image: '🍢' },
		{ id: 11, categoryId: 'korea', name: '삼계탕', price: 15000, image: '🐔' },
		{ id: 12, categoryId: 'korea', name: '육회', price: 28000, image: '🥩' },
		{ id: 13, categoryId: 'korea', name: '물냉면', price: 9000, image: '🍜' },
		{ id: 14, categoryId: 'korea', name: '보쌈', price: 32000, image: '🥬' },
		{ id: 15, categoryId: 'korea', name: '닭갈비', price: 25000, image: '🍳' },
		{ id: 16, categoryId: 'korea', name: '감자전', price: 10000, image: '🥔' },

		// --- 일식 (16개) ---
		{ id: 17, categoryId: 'japan', name: '초밥 세트', price: 15000, image: '🍣' },
		{ id: 18, categoryId: 'japan', name: '돈코츠 라멘', price: 9500, image: '🍜' },
		{ id: 19, categoryId: 'japan', name: '새우튀김 우동', price: 9000, image: '🍤' },
		{ id: 20, categoryId: 'japan', name: '돈까스 정식', price: 11000, image: '🍛' },
		{ id: 21, categoryId: 'japan', name: '가츠동', price: 9000, image: '덮밥' },
		{ id: 22, categoryId: 'japan', name: '사케동', price: 12000, image: '🐟' },
		{ id: 23, categoryId: 'japan', name: '모듬회', price: 35000, image: ' sashimi' },
		{ id: 24, categoryId: 'japan', name: '메밀소바', price: 8500, image: 'そば' },
		{ id: 25, categoryId: 'japan', name: '오코노미야끼', price: 14000, image: '🥞' },
		{ id: 26, categoryId: 'japan', name: '타코야끼', price: 7000, image: '🐙' },
		{ id: 27, categoryId: 'japan', name: '야키토리', price: 16000, image: '🍢' },
		{ id: 28, categoryId: 'japan', name: '규동', price: 9500, image: '🐮' },
		{ id: 29, categoryId: 'japan', name: '미소라멘', price: 9000, image: '🍜' },
		{ id: 30, categoryId: 'japan', name: '텐동', price: 11000, image: '🍚' },
		{ id: 31, categoryId: 'japan', name: '장어덮밥', price: 20000, image: '鰻' },
		{ id: 32, categoryId: 'japan', name: '카레라이스', price: 8500, image: '🍛' },

		// --- 중식 (16개) ---
		{ id: 33, categoryId: 'china', name: '짜장면', price: 7000, image: '🍝' },
		{ id: 34, categoryId: 'china', name: '짬뽕', price: 8000, image: '🍜' },
		{ id: 35, categoryId: 'china', name: '탕수육', price: 18000, image: '🍖' },
		{ id: 36, categoryId: 'china', name: '마파두부', price: 9000, image: '🍲' },
		{ id: 37, categoryId: 'china', name: '양장피', price: 28000, image: '🥗' },
		{ id: 38, categoryId: 'china', name: '깐풍기', price: 22000, image: '🍗' },
		{ id: 39, categoryId: 'china', name: '고추잡채', price: 25000, image: '🌶️' },
		{ id: 40, categoryId: 'china', name: '유산슬', price: 26000, image: '🥣' },
		{ id: 41, categoryId: 'china', name: '계란 볶음밥', price: 7500, image: '🍚' },
		{ id: 42, categoryId: 'china', name: '울면', price: 8500, image: '🍜' },
		{ id: 43, categoryId: 'china', name: '동파육', price: 30000, image: '🥩' },
		{ id: 44, categoryId: 'china', name: '어향가지', price: 18000, image: '🍆' },
		{ id: 45, categoryId: 'china', name: '칠리새우', price: 24000, image: '🍤' },
		{ id: 46, categoryId: 'china', name: '멘보샤', price: 15000, image: '🍞' },
		{ id: 47, categoryId: 'china', name: '마라탕', price: 12000, image: '🌶️' },
		{ id: 48, categoryId: 'china', name: '딤섬', price: 8000, image: '🥟' },

		// --- 양식 (16개) ---
		{ id: 49, categoryId: 'western', name: '토마토 파스타', price: 12000, image: '🍝' },
		{ id: 50, categoryId: 'western', name: '크림 리조또', price: 13000, image: '🍚' },
		{ id: 51, categoryId: 'western', name: '고르곤졸라 피자', price: 16000, image: '🍕' },
		{ id: 52, categoryId: 'western', name: '함박 스테이크', price: 14000, image: '🍳' },
		{ id: 53, categoryId: 'western', name: '봉골레 파스타', price: 12500, image: '🍝' },
		{ id: 54, categoryId: 'western', name: '마르게리타 피자', price: 15000, image: '🍕' },
		{ id: 55, categoryId: 'western', name: '안심 스테이크', price: 38000, image: '🥩' },
		{ id: 56, categoryId: 'western', name: '시저 샐러드', price: 9500, image: '🥗' },
		{ id: 57, categoryId: 'western', name: '감바스 알 아히요', price: 14000, image: '🍤' },
		{ id: 58, categoryId: 'western', name: '라자냐', price: 15000, image: ' lasagna' },
		{ id: 59, categoryId: 'western', name: '클럽 샌드위치', price: 10000, image: '🥪' },
		{ id: 60, categoryId: 'western', name: '에그 베네딕트', price: 13000, image: '🥚' },
		{ id: 61, categoryId: 'western', name: '버섯 크림 스프', price: 6000, image: '🥣' },
		{ id: 62, categoryId: 'western', name: '바베큐 폭립', price: 29000, image: '🍖' },
		{ id: 63, categoryId: 'western', name: '로제 파스타', price: 13500, image: '🍝' },
		{ id: 64, categoryId: 'western', name: '알리오 올리오', price: 11000, image: '🍝' },
	];
	let selectedCategoryId = 'korea'; let cart = [];
	$: filteredMenu = menuItems.filter(item => item.categoryId === selectedCategoryId);
	$: cartTotal = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
	function selectCategory(categoryId) { selectedCategoryId = categoryId; }
	function addToCart(menuItem) { const existingItem = cart.find(item => item.id === menuItem.id); if (existingItem) { existingItem.quantity++; cart = cart; } else { cart = [...cart, { ...menuItem, quantity: 1 }]; } }
	function adjustQuantity(itemId, amount) { const itemIndex = cart.findIndex(item => item.id === itemId); if (itemIndex === -1) return; cart[itemIndex].quantity += amount; if (cart[itemIndex].quantity <= 0) { cart = cart.filter(item => item.id !== itemId); } else { cart = cart; } }
	function clearCart() { cart = []; }
	function goToPayment() { if (cart.length === 0) { return; } cartStore.set(cart); cartTotalStore.set(cartTotal); goto('/payment'); }
</script>

<!-- 페이지 전체 레이아웃 (스크롤 방지) -->
<div class="page-layout">
	<!-- 1. 왼쪽: 메뉴 선택 영역 -->
	<div class="menu-section">
		<nav class="category-nav">
			{#each categories as category}
				<button class="category-button" class:active={selectedCategoryId === category.id} on:click={() => selectCategory(category.id)}>{category.name}</button>
			{/each}
		</nav>
		<!-- 메뉴 목록만 스크롤되는 영역 -->
		<div class="menu-grid">
			{#each filteredMenu as item (item.id)}
				<button class="menu-item" on:click={() => addToCart(item)}>
					<div class="menu-item-image">{item.image}</div>
					<div class="menu-item-name">{item.name}</div>
					<div class="menu-item-price">{item.price.toLocaleString()}원</div>
				</button>
			{/each}
		</div>
	</div>

	<!-- 2. 오른쪽: 주문 내역 영역 -->
	<div class="cart-section">
		<h2 class="cart-title">주문 내역</h2>
		<div class="cart-items-box">
			{#if cart.length === 0}
				<div class="cart-empty"><span>메뉴를 선택해주세요</span></div>
			{:else}
				{#each cart as item (item.id)}
					<div class="cart-item">
						<div class="item-info">
							<span class="item-name">{item.name}</span>
							<span class="item-price">{(item.price * item.quantity).toLocaleString()}원</span>
						</div>
						<div class="quantity-control">
							<button class="quantity-btn" on:click={() => adjustQuantity(item.id, -1)}>−</button>
							<span class="quantity-display">{item.quantity}</span>
							<button class="quantity-btn" on:click={() => adjustQuantity(item.id, 1)}>+</button>
						</div>
					</div>
				{/each}
			{/if}
		</div>
		<div class="cart-summary">
			<div class="total-amount"><span>총 주문금액</span><span class="total-price">{cartTotal.toLocaleString()}원</span></div>
			<div class="cart-actions">
				<button class="action-btn clear-btn" on:click={clearCart}>전체 취소</button>
				<button class="action-btn pay-btn" on:click={goToPayment}>결제하기</button>
			</div>
		</div>
	</div>
</div>

<style>
	/* --- ▼▼▼ 핵심 수정 부분 ▼▼▼ --- */
	.page-layout {
		display: grid;
		grid-template-columns: minmax(0, 2fr) minmax(0, 1fr);
		gap: 2rem;
		width: 100%;
		height: 100%; /* 부모(content-area)의 높이를 꽉 채움 */
		padding: 2.5rem;
		box-sizing: border-box;
	}

	.menu-section {
		background: #ffffff;
		border-radius: 24px;
		/* 핵심: 내부 요소를 수직으로 배치하고, 스크롤 영역을 제어하기 위함 */
		display: flex;
		flex-direction: column;
		padding: 2.5rem;
		height: 100%; /* 그리드 셀의 높이를 꽉 채움 */
		box-sizing: border-box;
		overflow: hidden; /* 내부 스크롤이 밖으로 영향을 주지 않도록 */
	}

	.category-nav {
		flex-shrink: 0; /* 카테고리 바의 높이는 고정 */
		display: flex;
		gap: 0.75rem;
	}
	.category-button {
		padding: 0.7rem 1.4rem;
		font-size: 1.1rem;
		font-weight: 500;
		border: 1px solid #dee2e6;
		background-color: #fff;
		color: #495057;
		border-radius: 8px;
		cursor: pointer;
	}
	.category-button.active {
		background-color: #343a40;
		color: white;
		border-color: #343a40;
		font-weight: 600;
	}

	.menu-grid {
		margin-top: 1.5rem;
		flex-grow: 1; /* 핵심: 남는 모든 공간을 차지 */
		overflow-y: auto; /* 핵심: 내용이 넘치면 스크롤바 생성 */
		padding-right: 1rem;
		margin-right: -1rem;
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
		gap: 1.25rem;
	}
	/* --- ▲▲▲ 핵심 수정 부분 끝 ▲▲▲ --- */

	.menu-item {
		background: #f8f9fa; border: 1px solid #f1f3f5; border-radius: 16px;
		padding: 1rem; text-align: center; cursor: pointer; transition: all 0.2s; 
		display: flex; flex-direction: column; justify-content: center; align-items: center; 
		gap: 0.5rem;
	}
	.menu-item:hover { transform: translateY(-5px); border-color: #ced4da; }
	.menu-item-image { font-size: 3.5rem; line-height: 1; }
	.menu-item-name { font-size: 1.1rem; font-weight: 600; color: #343a40; }
	.menu-item-price { font-size: 1rem; font-weight: 500; color: #868e96; }

	.cart-section {
		background-color: #ffffff; border-radius: 24px; padding: 2.5rem; box-sizing: border-box; height: 100%; display: flex; flex-direction: column; overflow: hidden;
	}
	.cart-title {
		flex-shrink: 0; font-size: 1.8rem; font-weight: 600; text-align: center; color: #343a40; margin: 0 0 1.5rem 0;
	}
	.cart-items-box {
		flex-grow: 1; overflow-y: auto; padding: 0 0.5rem;
	}
	.cart-empty { height: 100%; display: flex; justify-content: center; align-items: center; color: #adb5bd; font-size: 1.2rem; }
	.cart-item { display: flex; justify-content: space-between; align-items: center; padding: 1rem 0; border-bottom: 1px solid #f1f3f5; }
	.item-info { display: flex; flex-direction: column; gap: 0.3rem; }
	.item-name { font-size: 1.1rem; font-weight: 500; color: #212529; }
	.item-price { font-size: 1rem; color: #868e96; }
	.quantity-control { display: flex; align-items: center; gap: 1rem; }
	.quantity-display { font-size: 1.2rem; font-weight: 600; min-width: 20px; text-align: center; color: #212529; }
	.quantity-btn { width: 32px; height: 32px; border-radius: 50%; border: 1px solid #dee2e6; background-color: #fff; font-size: 1.5rem; line-height: 1; cursor: pointer; color: #495057; display: flex; justify-content: center; align-items: center; padding: 0; transition: background-color 0.15s; }
	.quantity-btn:hover { background-color: #f1f3f5; border-color: #adb5bd; }
	.cart-summary { flex-shrink: 0; padding-top: 1.5rem; border-top: 1px solid #f1f3f5; }
	.total-amount { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 1.5rem; }
	.total-amount span { font-size: 1.2rem; font-weight: 600; color: #495057; }
	.total-amount .total-price { font-size: 1.8rem; font-weight: 700; color: #d9480f; }
	.cart-actions { display: grid; grid-template-columns: 1fr 2fr; gap: 1rem; }
	.action-btn { padding: 1.2rem; font-size: 1.2rem; font-weight: 700; border: none; border-radius: 12px; cursor: pointer; }
	.clear-btn { background-color: #e9ecef; color: #495057; }
	.pay-btn { background-color: #1c7ed6; color: white; }
</style>