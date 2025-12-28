<script>
    let { onLoginSuccess } = $props();

    let isLogin = $state(true);
    let Token = $state("");
    let tokenAfterFitching = $state("");
    let getauthcode = () => {
        my.getAuthCode({
            scopes: ["auth_base", "USER_ID"],
            success: (res) => {
                Token = res.authCode;
                isLogin = true;
                fetch("https://its.mouamle.space/api/auth-with-superQi", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify({
                        token: Token,
                    }),
                })
                    .then((res) => res.json())
                    .then((data) => {
                        tokenAfterFitching = data.token;
                        my.alert({
                            content: "Login successful",
                        });
                        localStorage.setItem("token", tokenAfterFitching);
                        if (onLoginSuccess) onLoginSuccess();
                    })
                    .catch((err) => {
                        my.alert({
                            content: "Login failed" + err,
                        });
                    });
            },
        });
    };
</script>

<div class="page">
    <div class="content">
        <div class="login-container">
            <h2 class="title">Welcome Back</h2>
            <button class="auth-btn" onclick={getauthcode}>Login</button>
        </div>
    </div>
</div>

<style>
    .page {
        height: 100vh;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #f3f4f6;
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
            Helvetica, Arial, sans-serif;
    }

    .content {
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .login-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: 2rem;
        background-color: white;
        border-radius: 12px;
        box-shadow:
            0 4px 6px -1px rgba(0, 0, 0, 0.1),
            0 2px 4px -1px rgba(0, 0, 0, 0.06);
        width: 100%;
        max-width: 320px;
    }

    .title {
        margin-bottom: 2rem;
        color: #1f2937;
        font-size: 1.5rem;
        font-weight: 600;
        text-align: center;
    }

    .auth-btn {
        width: 100%;
        padding: 12px 24px;
        font-size: 16px;
        font-weight: 500;
        background-color: #2563eb;
        color: white;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        transition: all 0.2s ease;
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    }

    .auth-btn:hover {
        background-color: #1d4ed8;
        transform: translateY(-1px);
        box-shadow: 0 4px 6px -1px rgba(37, 99, 235, 0.2);
    }

    .auth-btn:active {
        transform: translateY(0);
    }
</style>
