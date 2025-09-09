<!-- src/routes/payment/+page.svelte (레이아웃 최종 수정) -->

<script>
    import { goto } from '$app/navigation';
    import { cart, cartTotal } from '$lib/cartStore.js';

    // (스크립트 내용은 이전과 동일합니다)
    let paymentStep = 'selection';
    let phoneNumber = '';
    let pointsApplied = false;
    let orderNumber = 0;

    function handleKeypadInput(num) { if (phoneNumber.length < 13) { phoneNumber += num; if (phoneNumber.length === 3 || phoneNumber.length === 8) { phoneNumber += '-'; } } }
    function clearPhoneNumber() { phoneNumber = phoneNumber.slice(0, -1); }
    function confirmPoints() { if (phoneNumber.length < 12) { alert('올바른 전화번호를 입력해주세요.'); return; } pointsApplied = true; paymentStep = 'selection'; }
    function handlePayment(method) { paymentStep = 'processing'; setTimeout(() => { const isSuccess = Math.random() > 0.1; if (isSuccess) { paymentStep = 'success'; orderNumber = Math.floor(Math.random() * 900) + 100; setTimeout(() => { goto('/'); }, 5000); } else { paymentStep = 'failure'; } }, 3000); }
    function retryPayment() { paymentStep = 'selection'; pointsApplied = false; }
</script>

<!-- 이제 이 페이지도 레이아웃의 kiosk-frame 안에 들어가게 됩니다. -->
<!-- 1. 주문 확인 & 포인트/결제 선택 -->
{#if paymentStep === 'selection'}
    <div class="page-layout">
        <h1 class="title">주문 확인 및 결제</h1>
        
        <div class="order-summary">
            <div class="summary-header"><span>주문 메뉴</span><span>수량</span></div>
            <div class="summary-items">
                {#each $cart as item}
                    <div class="summary-item"><span>{item.name}</span><span>{item.quantity}</span></div>
                {/each}
            </div>
            <div class="summary-total">
                <span>총 결제금액</span>
                <span class="total-price">{$cartTotal.toLocaleString()}원</span>
            </div>
        </div>

        {#if pointsApplied}
            <div class="points-applied-message">
                <span>✔</span> {phoneNumber} 님, 포인트가 적립됩니다.
            </div>
        {/if}

        <div class="main-actions">
            {#if !pointsApplied}
                <button class="action-btn secondary-btn" on:click={() => paymentStep = 'earningPoints'}>포인트 적립</button>
            {/if}
            <button class="action-btn primary-btn" on:click={() => handlePayment('카드')}>💳 신용/체크카드 결제</button>
            <button class="action-btn qr-btn" on:click={() => handlePayment('QR')}>📷 QR/바코드 결제</button>
        </div>
        <button class="cancel-btn" on:click={() => goto('/order')}>&larr; 주문 수정하기</button>
    </div>

<!-- 2. 포인트 적립 화면 -->
{:else if paymentStep === 'earningPoints'}
    <div class="page-layout">
        <h1 class="title">포인트 적립</h1>
        <p class="subtitle">휴대폰 번호를 입력해주세요.</p>
        <div class="phone-display">{phoneNumber || '010-XXXX-XXXX'}</div>
        <div class="keypad">
            {#each [1, 2, 3, 4, 5, 6, 7, 8, 9, '←', 0, '확인'] as key}
                {#if key === '확인'}<button class="keypad-btn confirm-btn" on:click={confirmPoints}>{key}</button>
                {:else if key === '←'}<button class="keypad-btn" on:click={clearPhoneNumber}>{key}</button>
                {:else}<button class="keypad-btn" on:click={() => handleKeypadInput(key)}>{key}</button>{/if}
            {/each}
        </div>
        <button class="cancel-btn" on:click={() => paymentStep = 'selection'}>적립하지 않고 결제하기</button>
    </div>

<!-- 기타 상태 화면들 -->
{:else}
    <div class="status-screen">
        {#if paymentStep === 'processing'}
            <div class="spinner"></div>
            <h2 class="status-title">결제를 진행중입니다</h2>
            <p class="status-message">카드를 아래 투입구에 끝까지 넣어주세요.</p>
        {:else if paymentStep === 'success'}
            <div class="icon success-icon">✔</div>
            <h2 class="status-title success-title">결제가 완료되었습니다!</h2>
            {#if pointsApplied}<p class="status-message points-applied-final">포인트가 정상적으로 적립되었습니다.</p>{/if}
            <div class="order-number-box"><p>주문번호</p><strong class="order-number">{orderNumber}</strong></div>
            <p class="status-message small">잠시 후 처음 화면으로 돌아갑니다.</p>
        {:else if paymentStep === 'failure'}
            <div class="icon failure-icon">✖</div>
            <h2 class="status-title failure-title">결제에 실패했습니다</h2>
            <p class="status-message">카드 정보를 확인해주세요.</p>
            <div class="failure-actions"><button class="action-btn secondary-btn" on:click={retryPayment}>다시 시도</button></div>
        {/if}
    </div>
{/if}


<style>
    .page-layout {
        width: 100%;
        height: 100%;
        padding: 2rem 2.5rem;
        box-sizing: border-box;
        /* 핵심: flexbox를 사용하여 전체 수직 레이아웃을 제어합니다. */
        display: flex;
        flex-direction: column;
    }
    .title {
        flex-shrink: 0; /* 높이 고정 */
        font-size: 2.2rem; text-align: center; margin: 0 0 2rem 0; color: #343a40;
    }
    .subtitle { 
        flex-shrink: 0; font-size: 1.4rem; color: #868e96; text-align: center; margin: -1.5rem 0 2rem 0; 
    }
    
    /* --- 주문확인 --- */
    .order-summary {
        flex-shrink: 0; /* 높이가 내용에 따라 결정되지만, 늘어나지는 않음 */
        border: 1px solid #e9ecef; border-radius: 16px; margin-bottom: 1.5rem;
        /* 핵심: 내부 스크롤을 위해 flexbox 컨테이너로 만듦 */
        display: flex;
        flex-direction: column;
        /* 중요: 내용이 많아도 이 박스 자체가 커지는 것을 막음 */
        max-height: 50%;
    }
    .summary-header {
        flex-shrink: 0;
        display: flex; justify-content: space-between; padding: 1rem 1.5rem; background-color: #f8f9fa; border-bottom: 1px solid #e9ecef; font-weight: 600; color: #868e96;
    }
    .summary-items {
        flex: 1; /* 핵심: 남는 공간을 모두 차지하고, 내용이 넘치면 스크롤 */
        overflow-y: auto; 
        padding: 0 1.5rem;
    }
    .summary-item {
        display: flex; justify-content: space-between; padding: 1rem 0; font-size: 1.1rem;
    }
    .summary-total {
        flex-shrink: 0;
        display: flex; justify-content: space-between; align-items: baseline; padding: 1.2rem 1.5rem; border-top: 1px solid #e9ecef; font-size: 1.3rem; font-weight: 600;
    }
    .total-price { color: #d9480f; font-size: 1.8rem; font-weight: 700; }
    
    .points-applied-message {
        flex-shrink: 0; text-align: center; padding: 1rem; background-color: #e6fcf5; color: #087f5b; border-radius: 12px; font-size: 1.1rem; font-weight: 600; margin-bottom: 1.5rem;
    }

    .main-actions { 
        flex-shrink: 0;
        display: flex; flex-direction: column; gap: 1rem; 
        margin-top: auto; /* 핵심: 버튼들을 항상 맨 아래로 밀어냄 */
    }
    .action-btn { padding: 1.5rem; font-size: 1.4rem; font-weight: 700; border: none; border-radius: 16px; cursor: pointer; }
    .primary-btn { background-color: #1c7ed6; color: white; }
    .secondary-btn { background-color: #f1f3f5; color: #495057; border: 1px solid #e9ecef; }
	.qr-btn { background-color: #f1f3f5; color: #495057; border: 1px solid #e9ecef;}
    .cancel-btn { 
        flex-shrink: 0; margin-top: 1.5rem; background: none; border: none; font-size: 1.1rem; color: #868e96; cursor: pointer; text-align: center; 
    }

    /* 포인트 적립 화면 */
    .phone-display { flex-shrink: 0; font-size: 2.5rem; text-align: center; background-color: #f8f9fa; padding: 1.5rem; border-radius: 16px; margin-bottom: 2rem; letter-spacing: 2px; color: #495057; }
    .keypad { flex: 1; display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem; }
    .keypad-btn { height: 100%; font-size: 2rem; font-weight: 600; border-radius: 16px; border: 1px solid #dee2e6; background-color: #f8f9fa; cursor: pointer; }
    .confirm-btn { grid-column: 3; grid-row: 4; background-color: #28a745; color: white; border: none;}

    /* 상태 화면 */
    .status-screen { flex: 1; display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; }
    .status-title { font-size: 2.5rem; margin: 2rem 0 1rem 0; }
    .status-message { font-size: 1.3rem; color: #495057; line-height: 1.6; }
    .status-message.small { font-size: 1rem; color: #adb5bd; margin-top: 2rem; }
    .points-applied-final { font-size: 1.1rem; color: #087f5b; background-color: #e6fcf5; padding: 0.5rem 1rem; border-radius: 8px; margin-top: -0.5rem; margin-bottom: 2rem; }
    .spinner { width: 60px; height: 60px; border: 6px solid #f1f3f5; border-top-color: #1c7ed6; border-radius: 50%; animation: spin 1s linear infinite; }
    @keyframes spin { to { transform: rotate(360deg); } }
    .icon { font-size: 4rem; width: 80px; height: 80px; border-radius: 50%; display: flex; justify-content: center; align-items: center; color: white; }
    .success-icon { background-color: #28a745; }
    .success-title { color: #28a745; }
    .failure-icon { background-color: #dc3545; padding-bottom: 8px;}
    .failure-title { color: #dc3545; }
    .order-number-box { margin-top: 2rem; background: #f8f9fa; padding: 1rem 2rem; border-radius: 16px; }
    .order-number-box p { margin: 0; font-size: 1.1rem; color: #868e96; }
    .order-number { font-size: 3rem; color: #1c7ed6; font-weight: 700; }
    .failure-actions { margin-top: 2rem; }
</style>