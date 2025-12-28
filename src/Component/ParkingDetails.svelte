<script>
    let {
        parkingName = "Unknown Parking",
        initialDuration = 2,
        hourlyRate = 2.5,
        onPay,
        onBack,
    } = $props();

    let duration = $state(initialDuration);

    let totalPrice = $derived((duration * hourlyRate).toFixed(2));

    function increaseDuration() {
        if (duration < 24) duration += 1;
    }

    function decreaseDuration() {
        if (duration > 1) duration -= 1;
    }
</script>

<div class="booking-card">
    <div class="header">
        <h3>Parking Details</h3>
    </div>

    <div class="parking-info">
        <div class="info-item">
            <span class="label">Location</span>
            <span class="value name">{parkingName}</span>
        </div>

        <div class="divider"></div>

        <div class="duration-control">
            <span class="label">Duration</span>
            <div class="stepper">
                <button
                    class="step-btn"
                    onclick={decreaseDuration}
                    disabled={duration <= 1}>−</button
                >
                <span class="duration-value">{duration} hrs</span>
                <button
                    class="step-btn"
                    onclick={increaseDuration}
                    disabled={duration >= 24}>+</button
                >
            </div>
        </div>

        <div class="info-item total">
            <span class="label">Total Price</span>
            <span class="value price">${totalPrice}</span>
        </div>
    </div>

    <div class="actions">
        <button class="pay-btn" onclick={onPay}>
            Confirm & Pay
        </button>
        <button class="back-btn" onclick={onBack}> Cancel </button>
    </div>
</div>

<style>
    .booking-card {
        background: white;
        width: 100%;
        max-width: 360px;
        padding: 30px;
        border-radius: 24px;
        box-shadow:
            0 20px 25px -5px rgba(0, 0, 0, 0.1),
            0 10px 10px -5px rgba(0, 0, 0, 0.04);
        display: flex;
        flex-direction: column;
        gap: 24px;
        animation: slideUp 0.4s cubic-bezier(0.16, 1, 0.3, 1);
    }

    .header h3 {
        margin: 0;
        font-size: 1.25rem;
        font-weight: 700;
        color: #111827;
        text-align: center;
    }

    .parking-info {
        display: flex;
        flex-direction: column;
        gap: 20px;
    }

    .info-item {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .label {
        font-size: 0.95rem;
        color: #6b7280;
        font-weight: 500;
    }

    .value.name {
        font-size: 1rem;
        font-weight: 600;
        color: #111827;
    }

    .divider {
        height: 1px;
        background-color: #f3f4f6;
        width: 100%;
    }

    /* Duration Stepper */
    .duration-control {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .stepper {
        display: flex;
        align-items: center;
        background: #f9fafb;
        border-radius: 12px;
        padding: 4px;
        border: 1px solid #e5e7eb;
    }

    .step-btn {
        width: 32px;
        height: 32px;
        display: flex;
        align-items: center;
        justify-content: center;
        border: none;
        background: white;
        border-radius: 8px;
        color: #374151;
        font-size: 1.2rem;
        cursor: pointer;
        box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
        transition: all 0.2s;
    }

    .step-btn:hover:not(:disabled) {
        background: #f3f4f6;
        color: #111827;
    }

    .step-btn:disabled {
        opacity: 0.5;
        cursor: not-allowed;
    }

    .duration-value {
        width: 60px;
        text-align: center;
        font-weight: 600;
        color: #111827;
        font-size: 0.95rem;
    }

    .total .value.price {
        font-size: 1.5rem;
        color: #4f46e5;
        font-weight: 800;
    }

    /* Actions */
    .actions {
        display: flex;
        flex-direction: column;
        gap: 12px;
    }

    .pay-btn {
        width: 100%;
        padding: 14px;
        font-size: 1rem;
        font-weight: 600;
        cursor: pointer;
        border: none;
        border-radius: 12px;
        background: linear-gradient(135deg, #4f46e5 0%, #4338ca 100%);
        color: white;
        box-shadow: 0 4px 6px -1px rgba(79, 70, 229, 0.2);
        transition:
            transform 0.2s,
            box-shadow 0.2s;
    }

    .pay-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 10px 15px -3px rgba(79, 70, 229, 0.3);
    }

    .pay-btn:active {
        transform: translateY(0);
    }

    .back-btn {
        width: 100%;
        padding: 12px;
        font-size: 0.95rem;
        font-weight: 500;
        cursor: pointer;
        border: none;
        background: transparent;
        color: #6b7280;
        transition: color 0.2s;
    }

    .back-btn:hover {
        color: #374151;
        text-decoration: underline;
    }

    @keyframes slideUp {
        from {
            opacity: 0;
            transform: translateY(20px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }
</style>
