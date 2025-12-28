<script>
    let isLogin = $state(true)
    let Token = $state("")
    let tokenAfterFitching = $state("")
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
                        tokenAfterFitching = data.token
                        my.alert({
                            content: "Login successful",
                        });
                        localStorage.setItem("token", tokenAfterFitching);
                    })
                    .catch((err) => {
                        let errorDetails = "";
                        if (err && typeof err === "object") {
                            errorDetails = JSON.stringify(err, null, 2);
                        } else {
                            errorDetails = String(err);
                        }
                        my.alert({
                            content: "Error: " + errorDetails,
                        });
                    });
            },
        });
    };
</script>

<div class="page">
    <div class="content">
    
        <button class="auth-btn" onclick={getauthcode}>Login</button>
        
    </div>
</div>

<style>
    .page {
        height: 100vh;
        display: flex;
        flex-direction: column;
    }

    .content {
        flex: 1;
    }

    .auth-btn {
        padding: 15px;
        font-size: 16px;
        background-color: #2563eb;
        color: white;
        border: none;
        cursor: pointer;
    }

    .auth-btn:hover {
        background-color: #1e40af;
    }
</style>
