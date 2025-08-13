<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login & Register</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        /* Animated background styles */
        .animated-bg {
            background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
            background-size: 400% 400%;
            animation: gradient 15s ease infinite;
            height: 100vh;
            width: 100vw;
            position: fixed;
            top: 0;
            left: 0;
            z-index: -1;
        }

        @keyframes gradient {
            0% {
                background-position: 0% 50%;
            }
            50% {
                background-position: 100% 50%;
            }
            100% {
                background-position: 0% 50%;
            }
        }

        /* Custom styles for form elements */
        .form-container {
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .error-message {
            color: #f87171; /* red-400 */
            font-size: 0.875rem;
            margin-top: 0.25rem;
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen bg-gray-900">
    <div class="animated-bg"></div>

    <div id="main-container" class="w-full max-w-md p-8 space-y-6 rounded-xl form-container shadow-lg">

        <!-- Login Form -->
        <div id="login-form">
            <h1 class="text-3xl font-bold text-center text-white">Welcome Back!</h1>
            <p class="text-center text-gray-200">Sign in to continue</p>
            <form id="login" class="mt-8 space-y-6">
                <div>
                    <label for="login-name" class="sr-only">Name</label>
                    <input id="login-name" name="name" type="text" required class="relative block w-full px-3 py-3 text-white bg-transparent border border-gray-300 rounded-md appearance-none placeholder:text-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm" placeholder="Name">
                    <p id="login-name-error" class="error-message hidden"></p>
                </div>
                <div>
                    <label for="login-password" class="sr-only">Password</label>
                    <input id="login-password" name="password" type="password" required class="relative block w-full px-3 py-3 text-white bg-transparent border border-gray-300 rounded-md appearance-none placeholder:text-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm" placeholder="Password">
                     <p id="login-password-error" class="error-message hidden"></p>
                </div>

                <div class="flex items-center justify-between">
                    <div class="text-sm">
                        <a href="#" id="forgot-password-link" class="font-medium text-indigo-300 hover:text-indigo-400">Forgot your password?</a>
                    </div>
                </div>

                <div>
                    <button type="submit" class="relative flex justify-center w-full px-4 py-3 text-sm font-medium text-white bg-indigo-600 border border-transparent rounded-md group hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        Sign in
                    </button>
                </div>
            </form>
            <p class="mt-6 text-sm text-center text-gray-300">
                Not a member?
                <a href="#" id="show-register" class="font-medium text-indigo-300 hover:text-indigo-400">Sign up now</a>
            </p>
        </div>

        <!-- Registration Form -->
        <div id="register-form" class="hidden">
            <h1 class="text-3xl font-bold text-center text-white">Create Account</h1>
            <p class="text-center text-gray-200">Get started with your new account</p>
            <form id="register" class="mt-8 space-y-4">
                <input id="register-name" name="name" type="text" placeholder="Name" class="relative block w-full px-3 py-3 text-white bg-transparent border border-gray-300 rounded-md appearance-none placeholder:text-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                <p id="register-name-error" class="error-message hidden"></p>

                <input id="register-password" name="password" type="password" placeholder="Create Password" class="relative block w-full px-3 py-3 text-white bg-transparent border border-gray-300 rounded-md appearance-none placeholder:text-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                <p id="register-password-error" class="error-message hidden"></p>

                <input id="register-phone" name="phone" type="tel" placeholder="Phone Number" class="relative block w-full px-3 py-3 text-white bg-transparent border border-gray-300 rounded-md appearance-none placeholder:text-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                <p id="register-phone-error" class="error-message hidden"></p>

                <input id="register-email" name="email" type="email" placeholder="Email Address" class="relative block w-full px-3 py-3 text-white bg-transparent border border-gray-300 rounded-md appearance-none placeholder:text-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                <p id="register-email-error" class="error-message hidden"></p>

                <input id="register-address" name="address" type="text" placeholder="Address" class="relative block w-full px-3 py-3 text-white bg-transparent border border-gray-300 rounded-md appearance-none placeholder:text-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                <p id="register-address-error" class="error-message hidden"></p>

                <div>
                    <button type="submit" class="relative flex justify-center w-full px-4 py-3 mt-4 text-sm font-medium text-white bg-indigo-600 border border-transparent rounded-md group hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        Sign up
                    </button>
                </div>
            </form>
            <p class="mt-6 text-sm text-center text-gray-300">
                Already have an account?
                <a href="#" id="show-login" class="font-medium text-indigo-300 hover:text-indigo-400">Sign in</a>
            </p>
        </div>
        
        <!-- Forgot Password Modal -->
        <div id="forgot-password-modal" class="hidden">
             <h1 class="text-3xl font-bold text-center text-white">Forgot Password</h1>
             <p class="text-center text-gray-200">Enter your email to reset your password</p>
             <form id="forgot-password-form" class="mt-8 space-y-6">
                 <input id="forgot-email" name="email" type="email" required class="relative block w-full px-3 py-3 text-white bg-transparent border border-gray-300 rounded-md appearance-none placeholder:text-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm" placeholder="Email Address">
                 <p id="forgot-email-error" class="error-message hidden"></p>
                 <div>
                    <button type="submit" class="relative flex justify-center w-full px-4 py-3 text-sm font-medium text-white bg-indigo-600 border border-transparent rounded-md group hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        Send Reset Link
                    </button>
                </div>
             </form>
             <p class="mt-6 text-sm text-center text-gray-300">
                Remembered your password?
                <a href="#" id="back-to-login" class="font-medium text-indigo-300 hover:text-indigo-400">Back to Sign In</a>
            </p>
        </div>

        <!-- Success Message Box -->
        <div id="message-box" class="hidden p-4 mt-4 text-center text-white bg-green-500 rounded-md"></div>

    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // --- DOM Elements ---
            const loginFormContainer = document.getElementById('login-form');
            const registerFormContainer = document.getElementById('register-form');
            const forgotPasswordModal = document.getElementById('forgot-password-modal');
            const messageBox = document.getElementById('message-box');

            const showRegisterLink = document.getElementById('show-register');
            const showLoginLink = document.getElementById('show-login');
            const forgotPasswordLink = document.getElementById('forgot-password-link');
            const backToLoginLink = document.getElementById('back-to-login');

            const loginForm = document.getElementById('login');
            const registerForm = document.getElementById('register');
            const forgotPasswordForm = document.getElementById('forgot-password-form');

            // --- Toggling Functions ---
            const showForm = (formToShow) => {
                loginFormContainer.classList.add('hidden');
                registerFormContainer.classList.add('hidden');
                forgotPasswordModal.classList.add('hidden');
                formToShow.classList.remove('hidden');
            };

            showRegisterLink.addEventListener('click', (e) => {
                e.preventDefault();
                showForm(registerFormContainer);
            });

            showLoginLink.addEventListener('click', (e) => {
                e.preventDefault();
                showForm(loginFormContainer);
            });
            
            forgotPasswordLink.addEventListener('click', (e) => {
                e.preventDefault();
                showForm(forgotPasswordModal);
            });

            backToLoginLink.addEventListener('click', (e) => {
                e.preventDefault();
                showForm(loginFormContainer);
            });

            // --- Validation and Message Functions ---
            const showError = (inputId, message) => {
                const errorElement = document.getElementById(`${inputId}-error`);
                errorElement.textContent = message;
                errorElement.classList.remove('hidden');
                document.getElementById(inputId).classList.add('border-red-400');
            };

            const clearError = (inputId) => {
                const errorElement = document.getElementById(`${inputId}-error`);
                errorElement.classList.add('hidden');
                 document.getElementById(inputId).classList.remove('border-red-400');
            };
            
            const clearAllErrors = (form) => {
                const inputs = form.querySelectorAll('input');
                inputs.forEach(input => clearError(input.id));
            }

            const showMessage = (message, isSuccess = true) => {
                messageBox.textContent = message;
                messageBox.className = `p-4 mt-4 text-center text-white rounded-md ${isSuccess ? 'bg-green-500' : 'bg-red-500'}`;
                messageBox.classList.remove('hidden');
                setTimeout(() => {
                    messageBox.classList.add('hidden');
                }, 3000);
            };
            
            const validateEmail = (email) => {
                const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
                return re.test(String(email).toLowerCase());
            }

            // --- Form Submit Handlers ---

            // Login Form Submission
            loginForm.addEventListener('submit', (e) => {
                e.preventDefault();
                clearAllErrors(loginForm);
                let isValid = true;

                const name = document.getElementById('login-name').value.trim();
                const password = document.getElementById('login-password').value.trim();

                if (name === '') {
                    showError('login-name', 'Name is required.');
                    isValid = false;
                }
                if (password === '') {
                    showError('login-password', 'Password is required.');
                    isValid = false;
                }

                if (isValid) {
                    // In a real app, you would send this to a server
                    console.log('Login attempt:', { name, password });
                    showMessage('Login successful! Redirecting...');
                    // Simulate redirect
                    setTimeout(() => {
                        loginForm.reset();
                        // window.location.href = '/dashboard'; // Example redirect
                    }, 1500);
                }
            });

            // Registration Form Submission
            registerForm.addEventListener('submit', (e) => {
                e.preventDefault();
                clearAllErrors(registerForm);
                let isValid = true;
                
                const name = document.getElementById('register-name').value.trim();
                const password = document.getElementById('register-password').value.trim();
                const phone = document.getElementById('register-phone').value.trim();
                const email = document.getElementById('register-email').value.trim();
                const address = document.getElementById('register-address').value.trim();

                if (name === '') {
                    showError('register-name', 'Name is required.');
                    isValid = false;
                }
                if (password.length < 8) {
                    showError('register-password', 'Password must be at least 8 characters.');
                    isValid = false;
                }
                 if (!/^\d{10,11}$/.test(phone)) { // Simple validation for 10-11 digit phone numbers
                    showError('register-phone', 'Please enter a valid phone number.');
                    isValid = false;
                }
                if (!validateEmail(email)) {
                    showError('register-email', 'Please enter a valid email address.');
                    isValid = false;
                }
                if (address === '') {
                    showError('register-address', 'Address is required.');
                    isValid = false;
                }

                if(isValid) {
                    // In a real app, you would send this to a server
                    console.log('Registration data:', { name, password, phone, email, address });
                    showMessage('Registration successful! Please sign in.');
                    setTimeout(() => {
                        registerForm.reset();
                        showForm(loginFormContainer);
                    }, 2000);
                }
            });
            
            // Forgot Password Form Submission
            forgotPasswordForm.addEventListener('submit', (e) => {
                e.preventDefault();
                clearAllErrors(forgotPasswordForm);
                let isValid = true;
                
                const email = document.getElementById('forgot-email').value.trim();
                
                if (!validateEmail(email)) {
                    showError('forgot-email', 'Please enter a valid email address.');
                    isValid = false;
                }
                
                if(isValid) {
                    // In a real app, you would send a reset link
                    console.log('Password reset request for:', email);
                    showMessage('If an account with that email exists, a reset link has been sent.');
                     setTimeout(() => {
                        forgotPasswordForm.reset();
                        showForm(loginFormContainer);
                    }, 2500);
                }
            });
        });
    </script>
</body>
</html>
