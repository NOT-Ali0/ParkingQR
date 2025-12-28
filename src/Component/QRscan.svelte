<script>
    import ParkingDetails from "./ParkingDetails.svelte";

    // "initial" | "details" | "payment"
    let scanStep = $state("initial");
    let parkName = $state("");

  

    const handleScan = () => {
            my.scan({
                type: "qr",
                success: (res) => {
                    parkName = res.code || "Unknown Parking";
                    scanStep = "details";
                },
                fail: (err) => {
                    my.alert({ content: "Scan failed. Please try again. " +err});
                },
           });
        
    };

    

    function handleBack() {
        scanStep = "initial";
        parkName = "";
    }

    function pay() {
        fetch("https://its.mouamle.space/api/payment", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                Authorization: `${localStorage.getItem("token")}`,
            },
        })
            .then((res) => res.json())
            .then((data) => {
                my.tradePay({
                    paymentUrl: data.url,
                    success: (res) => {
                        my.alert({
                            content: "Payment successful",
                        });
                        scanStep = "initial";
                    },
                });
            })
            .catch((err) => {
                my.alert({
                    content: "Payment failed" + err,
                });
            });
    }
</script>

<div class="background">
    {#if scanStep === "initial"}
        <div class="scan-container">
            <div class="scan-icon">
                <!-- Simple CSS Scan -->
                <div class="scan-frame"></div>
                <div class="scan-line"></div>
            </div>
            <h2 class="title">Scan to Park</h2>
            <p class="subtitle">Quick and easy parking payment</p>
            <button class="primary-btn" onclick={handleScan}>
                Scan QR Code
            </button>
        </div>
    {:else if scanStep === "details"}
        <ParkingDetails
            onPay={pay}
            parkingName={parkName}
            onBack={handleBack}
        />
    {/if}
</div>

<style>
    .background {
        height: 100vh;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        background: radial-gradient(circle at top left, #f3f4f6, #e5e7eb);
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
            Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
        color: #1f2937;
        padding: 20px;
        box-sizing: border-box;
    }

    .title {
        font-size: 1.5rem;
        font-weight: 700;
        margin-bottom: 0.5rem;
        color: #111827;
    }
    .subtitle {
        font-size: 1rem;
        color: #6b7280;
        margin-bottom: 2rem;
    }

    .primary-btn {
        width: 100%;
        max-width: 280px;
        padding: 14px 24px;
        font-size: 1rem;
        font-weight: 600;
        cursor: pointer;
        border: none;
        border-radius: 12px;
        background: linear-gradient(135deg, #4f46e5 0%, #4338ca 100%);
        color: white;
        box-shadow:
            0 4px 6px -1px rgba(79, 70, 229, 0.2),
            0 2px 4px -1px rgba(79, 70, 229, 0.1);
        transition:
            transform 0.2s cubic-bezier(0.4, 0, 0.2, 1),
            box-shadow 0.2s ease,
            filter 0.2s ease;
    }

    .primary-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 10px 15px -3px rgba(79, 70, 229, 0.3);
        filter: brightness(110%);
    }

    .primary-btn:active {
        transform: translateY(0);
    }

    /* Scan View */
    .scan-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
        animation: fadeIn 0.5s ease-out;
    }

    .scan-icon {
        position: relative;
        width: 120px;
        height: 120px;
        margin-bottom: 24px;
    }

    .scan-frame {
        width: 100%;
        height: 100%;
        border: 4px solid #4f46e5;
        border-radius: 20px;
        position: relative;
        box-sizing: border-box;
    }
    .scan-frame::before {
        content: "";
        position: absolute;
        top: -4px;
        left: 20px;
        right: 20px;
        height: 5px;
        background: #f3f4f6; 
        z-index: 1;
    }
    .scan-frame::after {
        content: "";
        position: absolute;
        bottom: -4px;
        left: 20px;
        right: 20px;
        height: 5px;
        background: #e5e7eb; 
        z-index: 1;
    }
    
    .scan-icon::before {
        content: "";
        position: absolute;
        top: 20px;
        bottom: 20px;
        left: -4px;
        width: 5px;
        background: radial-gradient(
            circle at top left,
            #f3f4f6,
            #e5e7eb
        ); 
        z-index: 1;
    }
    .scan-icon::after {
        content: "";
        position: absolute;
        top: 20px;
        bottom: 20px;
        right: -4px;
        width: 5px;
        background: radial-gradient(
            circle at top left,
            #f3f4f6,
            #e5e7eb
        ); 
        z-index: 1;
    }

    .scan-line {
        position: absolute;
        width: 80%;
        height: 2px;
        background: #fbbf24;
        left: 10%;
        top: 20%;
        z-index: 0;
        box-shadow: 0 0 10px #fbbf24;
        animation: scanMove 2s infinite ease-in-out;
    }

    @keyframes scanMove {
        0% {
            top: 20%;
            opacity: 0.5;
        }
        50% {
            top: 80%;
            opacity: 1;
        }
        100% {
            top: 20%;
            opacity: 0.5;
        }
    }

    @keyframes fadeIn {
        from {
            opacity: 0;
        }
        to {
            opacity: 1;
        }
    }
</style>
