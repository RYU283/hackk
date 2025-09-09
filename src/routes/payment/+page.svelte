<!-- src/routes/payment/+page.svelte (포인트 적립 기능 추가) -->

<script>
    import { goto } from '$app/navigation';
    import { cart, cartTotal } from '$lib/cartStore.js';

    // --- 상태 관리 ---
    // 'selection': 초기화면, 'earningPoints': 포인트적립, 'processing': 결제중, 'success': 성공, 'failure': 실패
    let paymentStep = 'selection';
    let phoneNumber = '';
    let pointsApplied = false; // 포인트 적립 여부
    let orderNumber = 0;

    // --- 키패드 관련 함수 ---
    function handleKeypadInput(num) {
        if (phoneNumber.length < 13) { // 010-XXXX-XXXX 형식
            phoneNumber += num;
            // 자동 하이픈(-) 추가
            if (phoneNumber.length === 3 || phoneNumber.length === 8) {
                phoneNumber += '-';
            }
        }
    }
    function clearPhoneNumber() {
        phoneNumber = phoneNumber.slice(0, -1);
    }

    // --- 포인트 적립 함수 ---
    function confirmPoints() {
        if (phoneNumber.length < 12) {
            alert('올바른 전화번호를 입력해주세요.');
            return;
        }
        console.log(`${phoneNumber}로 포인트 적립 시도...`);
        // (실제로는 여기서 서버와 통신하여 포인트를 적립합니다)
        pointsApplied = true;
        // 포인트 적립 후, 다시 결제 선택 화면으로 돌아갑니다.
        paymentStep = 'selection';
    }

    // --- 결제 관련 함수 ---
    function handlePayment(method) {
        paymentStep = 'processing';
        console.log(`${method} 결제 시작...`);

        setTimeout(() => {
            const isSuccess = Math.random() > 0.1;
            if (isSuccess) {
                paymentStep = 'success';
                orderNumber = Math.floor(Math.random() * 900) + 100;
                setTimeout(() => { goto('/'); }, 5000);
            } else {
                paymentStep = 'failure';
            }
        }, 3000);
    }

    function retryPayment() {
        paymentStep = 'selection';
        pointsApplied = false; // 실패 시 포인트 적립 상태 초기화
    }
</script>

<div class="page-container">
    <div class="payment-card">
        <!-- 1. 주문 확인 & 포인트/결제 선택 -->
        {#if paymentStep === 'selection'}
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

            <!-- 포인트 적립이 완료되면 메시지 표시 -->
            {#if pointsApplied}
                <div class="points-applied-message">
                    <span>✔</span> {phoneNumber} 님, 포인트가 적립됩니다.
                </div>
            {/if}

            <div class="main-actions">
                <!-- 포인트 적립 전이라면 '포인트 적립' 버튼 표시 -->
                {#if !pointsApplied}
                    <button class="action-btn secondary-btn" on:click={() => paymentStep = 'earningPoints'}>
                        포인트 적립
                    </button>
                {/if}
                <button class="action-btn primary-btn" on:click={() => handlePayment('카드')}>
                    💳 신용/체크카드 결제
                </button>
                 <button class="action-btn qr-btn" on:click={() => handlePayment('QR')}>
                    📷 QR/바코드 결제
                </button>
            </div>
            <button class="cancel-btn" on:click={() => goto('/order')}>
                &larr; 주문 수정하기
            </button>

        <!-- 2. 포인트 적립 화면 -->
        {:else if paymentStep === 'earningPoints'}
            <h1 class="title">포인트 적립</h1>
            <p class="subtitle">휴대폰 번호를 입력해주세요.</p>
            
            <div class="phone-display">{phoneNumber || '010-XXXX-XXXX'}</div>

            <div class="keypad">
                {#each [1, 2, 3, 4, 5, 6, 7, 8, 9, '←', 0, '확인'] as key}
                    {#if key === '확인'}
                        <button class="keypad-btn confirm-btn" on:click={confirmPoints}>{key}</button>
                    {:else if key === '←'}
                        <button class="keypad-btn" on:click={clearPhoneNumber}>{key}</button>
                    {:else}
                        <button class="keypad-btn" on:click={() => handleKeypadInput(key)}>{key}</button>
                    {/if}
                {/each}
            </div>
            <button class="cancel-btn" on:click={() => paymentStep = 'selection'}>
                적립하지 않고 결제하기
            </button>

        <!-- 3. 결제 진행중 화면 -->
        {:else if paymentStep === 'processing'}
            <div class="status-screen">
                <div class="spinner"></div>
                <h2 class="status-title">결제를 진행중입니다</h2>
                <p class="status-message">카드를 아래 투입구에 끝까지 넣어주세요.</p>
            </div>

        <!-- 4. 결제 성공 화면 -->
        {:else if paymentStep === 'success'}
            <div class="status-screen">
                <div class="icon success-icon">✔</div>
                <h2 class="status-title success-title">결제가 완료되었습니다!</h2>
                {#if pointsApplied}
                    <p class="status-message points-applied-final">포인트가 정상적으로 적립되었습니다.</p>
                {/if}
                <div class="order-number-box">
                    <p>주문번호</p>
                    <strong class="order-number">{orderNumber}</strong>
                </div>
                <p class="status-message small">잠시 후 처음 화면으로 돌아갑니다.</p>
            </div>

        <!-- 5. 결제 실패 화면 -->
        {:else if paymentStep === 'failure'}
            <div class="status-screen">
                <div class="icon failure-icon">✖</div>
                <h2 class="status-title failure-title">결제에 실패했습니다</h2>
                <p class="status-message">카드 정보를 확인해주세요.</p>
                <div class="failure-actions">
                    <button class="action-btn secondary-btn" on:click={retryPayment}>다시 시도</button>
                </div>
            </div>
        {/if}
    </div>
</div>

<style>
    :global(body, html) {
		margin: 0; padding: 0; height: 100vh; overflow: hidden; font-family: 'Pretendard', sans-serif; background-color: #f1f3f5;
	}
    .page-container {
		height: 100%; padding: 2.5vh; box-sizing: border-box; display: flex; justify-content: center; align-items: center;
	}
    .payment-card {
        width: 100%; max-width: 700px; height: 100%; background: #ffffff; border-radius: 24px; padding: 3rem; box-sizing: border-box; display: flex; flex-direction: column;
    }
    .title { font-size: 2.5rem; text-align: center; margin: 0 0 2rem 0; color: #343a40; }
    .subtitle { font-size: 1.4rem; color: #868e96; text-align: center; margin: -1.5rem 0 2rem 0; }
    
    .order-summary { border: 2px solid #e9ecef; border-radius: 16px; margin-bottom: 2rem; }
    .summary-header { display: flex; justify-content: space-between; padding: 1rem 1.5rem; background-color: #f8f9fa; border-bottom: 2px solid #e9ecef; font-weight: 600; color: #868e96; }
    .summary-items { max-height: 25vh; overflow-y: auto; padding: 0 1.5rem; }
    .summary-item { display: flex; justify-content: space-between; padding: 1rem 0; }
    .summary-total { display: flex; justify-content: space-between; align-items: baseline; padding: 1.5rem; border-top: 2px solid #e9ecef; font-size: 1.4rem; font-weight: 600; }
    .total-price { color: #d9480f; font-size: 2rem; font-weight: 700; }
    
    .points-applied-message { text-align: center; padding: 1rem; background-color: #e6fcf5; color: #087f5b; border-radius: 12px; font-size: 1.2rem; font-weight: 600; margin-bottom: 1.5rem; }
    .points-applied-message span { margin-right: 0.5rem; }

    .main-actions { display: flex; flex-direction: column; gap: 1rem; margin-top: auto; }
    .action-btn { padding: 1.75rem; font-size: 1.6rem; font-weight: 700; border: none; border-radius: 16px; cursor: pointer; }
    .primary-btn { background-color: #1c7ed6; color: white; }
    .secondary-btn { background-color: #e9ecef; color: #495057; }
	.qr-btn { background-color: #f8f9fa; color: #495057; border: 2px solid #e9ecef; }
    .cancel-btn { margin-top: 1.5rem; background: none; border: none; font-size: 1.2rem; color: #868e96; cursor: pointer; text-align: center; }

    /* 포인트 적립 화면 */
    .phone-display { font-size: 2.5rem; text-align: center; background-color: #f8f9fa; padding: 1.5rem; border-radius: 16px; margin-bottom: 2rem; letter-spacing: 2px; color: #495057; }
    .keypad { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem; margin-top: auto; }
    .keypad-btn { height: 80px; font-size: 2rem; font-weight: 600; border-radius: 16px; border: 1px solid #dee2e6; background-color: #f8f9fa; cursor: pointer; }
    .confirm-btn { grid-column: 3; grid-row: 4; background-color: #28a745; color: white; border: none;}

    /* 상태 화면 */
    .status-screen { flex: 1; display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; }
    .status-title { font-size: 3rem; margin: 2rem 0 1rem 0; }
    .status-message { font-size: 1.5rem; color: #495057; line-height: 1.6; }
    .status-message.small { font-size: 1.1rem; color: #adb5bd; margin-top: 2rem; }
    .points-applied-final { font-size: 1.2rem; color: #087f5b; background-color: #e6fcf5; padding: 0.5rem 1rem; border-radius: 8px; margin-top: -0.5rem; margin-bottom: 2rem; }
    .spinner { width: 80px; height: 80px; border: 8px solid #f1f3f5; border-top-color: #1c7ed6; border-radius: 50%; animation: spin 1s linear infinite; }
    @keyframes spin { to { transform: rotate(360deg); } }
    .icon { font-size: 5rem; width: 100px; height: 100px; border-radius: 50%; display: flex; justify-content: center; align-items: center; color: white; }
    .success-icon { background-color: #28a745; }
    .success-title { color: #28a745; }
    .failure-icon { background-color: #dc3545; padding-bottom: 10px;}
    .failure-title { color: #dc3545; }
    .order-number-box { margin-top: 2rem; background: #f8f9fa; padding: 1.5rem 3rem; border-radius: 16px; }
    .order-number-box p { margin: 0; font-size: 1.2rem; color: #868e96; }
    .order-number { font-size: 4rem; color: #1c7ed6; font-weight: 700; }
    .failure-actions { margin-top: 2rem; }
</style>