<script>
    import calculateStrength from './calculateStrength.js';
    import generatePassword from './generatePassword.js';

    let copied;
    let settings = {
        maxLength: 64,
        maxDigits: 10,
        maxSymbols: 10,
        length: 12,
        digits: 4,
        symbols: 2,
        ambiguous: true,
    };

    function copyToClipboard() {
        let copyElement = document.createElement("textarea");
        copyElement.style.opacity = '0';
        copyElement.style.position = 'fixed';
        copyElement.textContent = password;
        let body = document.getElementsByTagName('body')[0];
        body.appendChild(copyElement);
        copyElement.select();
        document.execCommand('copy');
        body.removeChild(copyElement);

        copied = true;
        setTimeout(() => {
            copied = false;
        }, 750);
    }

    function onFormKeydown(event) {
        if (event.code === 'Enter') {
            e.preventDefault();
        }
    }

    $: password = generatePassword(settings);
    $: strength = calculateStrength(password);
    $: lengthThumbPosition = ((settings.length - 6) / (settings.maxLength - 6)) * 100;
    $: digitsThumbPosition = ((settings.digits - 0) / (settings.maxDigits - 0)) * 100;
    $: symbolsThumbPosition = ((settings.symbols - 0) / (settings.maxSymbols - 0)) * 100;
</script>

<div id="app">
    <section class="wrapper">
        <h1>The Password Genie</h1>
        <div class="password-box">
            <span id="password" class="password" on:click={copyToClipboard}>{password}</span>
            <span class="regenerate-password" on:click={() => password = generatePassword(settings)}></span>
            <span class="copy-password" on:click={copyToClipboard}></span>
            {#if copied}
                <span class="tooltip">Password copied successfuly!</span>
            {/if}
        </div>
        <form on:keydown={onFormKeydown}>
            <div class="field-wrap">
                <label for="strength-slider">Strength</label>
                <span class="range-value">{strength.text}</span>
                <div class="range-slider_wrapper slider-strength {strength.text}">
                    <span class="slider-bar" style="width: {strength.score}%"></span>
                    <input id="strength-slider" type="range" class="range-slider" min="0" max="100" bind:value={strength.score} disabled>
                </div>
            </div>
            <div class="seperator"></div>
            <div class="field-wrap">
                <label for="length-slider">Length</label>
                <span class="range-value">{settings.length}</span>
                <div class="range-slider_wrapper">
                    <span class="slider-bar" style="width: {lengthThumbPosition}%"></span>
                    <input id="length-slider" type="range" class="range-slider" min="6" max={settings.maxLength} bind:value={settings.length}>
                </div>
            </div>
            <div class="field-wrap">
                <label for="digits-slider">Digits</label>
                <span class="range-value">{settings.digits}</span>
                <div class="range-slider_wrapper">
                    <span class="slider-bar" style="width: {digitsThumbPosition}"></span>
                    <input id="digits-slider" type="range" class="range-slider" min="0" max={settings.maxDigits} bind:value={settings.digits}>
                </div>
            </div>
            <div class="field-wrap">
                <label for="symbols-slider">Symbols</label>
                <span class="range-value">{settings.symbols}</span>
                <div class="range-slider_wrapper">
                    <span class="slider-bar" style="width: {symbolsThumbPosition}%"></span>
                    <input id="symbols-slider" type="range" class="range-slider" min="0" max={settings.maxSymbols} bind:value={settings.symbols}>
                </div>
            </div>
        </form>
    </section>
</div>

<footer>
    <b>Get the <a href="https://chrome.google.com/webstore/detail/the-password-genie/ccdnljkkehejmcfnmifnkpnodlikdehg?hl=en">Chrome extension</a></b>
    <div class="github-links">
        <a class="github-button" href="https://github.com/nourabusoud" aria-label="Follow @nourabusoud on GitHub">Follow @nourabusoud</a>
        <a class="github-button" href="https://github.com/nourabusoud/password-genie" data-icon="octicon-star" aria-label="Star nourabusoud/vue-todo-list on GitHub">Star</a>
    </div>
    Made with &lt;3 by <a href="https://www.nourabusoud.com/">Nour Soud</a><br>
    View on <a href="https://github.com/nourabusoud/password-genie">Github</a> or play around with it on <a href="https://codepen.io/nourabusoud/pen/YeMOVv">Codepen</a><br>
    Adapted for Svelte by <a href="https://github.com/chuckrector">Chuck Rector</a>
</footer>

<style lang="scss">
    $blue: #3fa4f4;
    $purple: #6e77f7;
    $peach: #fc8680;
    $pink: #ef5081;
    $green: #8BC34A;
    $dark-green: #4caf50;
    $red: #ff6666;
    $orange: #ff9800;

    $wrapper-bg: #f4f7fc;
    $wrapper-width: 400px;

    .wrapper {
        width: $wrapper-width;
        max-width: 100%;
        min-height: 400px;
        margin: 40px auto 20px;
        position: relative;
        border: 1px solid #eee;
        border-radius: 3px;
        padding: 40px 20px;
        font-size: 0.85em;
        box-shadow: 0 0 15px 0 rgba(0,0,0,0.05);
        background-color: $wrapper-bg;
        position: relative;
        transition: all ease-in 0.25s;
    }

    h1 {
        text-align: center;
        margin: 0 0 30px;
    }
    .field-wrap {
        margin-bottom: 20px;
    }

    form {
        overflow: overlay;
        margin-top: 30px;
    }

    label {
        display: inline-block;
        min-width: 20%;
    }

    .range-slider_wrapper {
        position: relative;
        width: 100%;
        margin: 10px 0 30px;
    }

    .range-slider {
        appearance: none;
        background: lighten($blue, 20%);
        width: 100%;
        border-radius: 3px;
        vertical-align: bottom;
        margin: 0;
        height: 6px;
        cursor: pointer;
        transition: all ease-in 0.25s;
    }

    .range-slider::-webkit-slider-thumb {
        appearance: none;
        border-radius: 0;
        border: 0;
        position:relative;
        width: 4px;
        height: 15px;
        background-color: darken($blue, 5%);
    }
    .range-slider::-moz-range-thumb {
        appearance: none;
        border-radius: 0;
        border: 0;
        position:relative;
        width: 4px;
        height: 15px;
        background-color: darken($blue, 5%);
    }

    .range-slider {
        &:focus {
            outline: none;
        }

        &:hover,
        &:active {
            &::-webkit-slider-thumb {
                top: 0px;
            }
        }
    }

    ::-moz-range-track {
        background: transparent;
        border: 0;
    }
    input::-moz-focus-inner,
    input::-moz-focus-outer {
        border: 0;
    }

    .range-value {
        text-transform: capitalize;
        float: right;
        vertical-align: bottom;
        min-width: 30px;
        text-align: center;
        border-radius: 3px;
        font-size: 0.9em;
    }

    .slider-bar {
        position: absolute;
        height: 6px;
        border-top-left-radius: 3px;
        border-bottom-left-radius: 3px;
        background: $blue;
        left: 0;
        bottom: 0;
        pointer-events: none;
    }

    .slider {
        &-strength {
            .range-slider {
                cursor: default;
            }

            .slider-bar {
                border-radius: 3px;
                transition: all ease-in 0.25s;
            }
            .range-slider::-webkit-slider-thumb {
                background-color: transparent;
            }
            .range-slider::-moz-range-thumb {
                background-color: transparent;
            }

            &.weak {
                .range-slider {
                    background-color: lighten($red, 30%);
                }
                .slider-bar, .slider-bar:after {
                    background-color: $red;
                }
            }

            &.average {
                .range-slider {
                    background-color: lighten($orange, 30%);
                }
                .slider-bar, .slider-bar:after {
                    background-color: $orange;
                }
            }

            &.strong{
                .range-slider {
                    background-color: lighten($green, 30%);
                }
                .slider-bar, .slider-bar:after {
                    background-color: $green;
                }
            }

            &.secure {
                .range-slider {
                    background-color: lighten($green, 30%);
                }
                .slider-bar,
                .slider-bar:after {
                    background-color: $green;
                }
            }
        }
    }

    .password-box {
        width: 100%;
        min-height: 80px;
        margin-bottom: 40px;
        position: relative;
        text-align: center;
        border-radius: 3px;
        background: #fff;
        letter-spacing: 2px;
        transition: all ease-in 0.3s;
        border: 1px solid rgb(189, 204, 230);

        .password {
            width: 70%;
            padding: 1.5em 1em;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            word-wrap: break-word;
        }
    }

    .regenerate-password,
    .copy-password {
        width: 44px;
        height: 50%;
        position: absolute;
        right: 0;
        transition: all ease-in 0.25s;

        &:hover {
            opacity: 0.8;
        }
    }

    .regenerate-password {
        top: 0;
        background-color: #fff;
        background-image: url('/regenerate.svg');
        background-size: 40%;
        background-position: center center;
        background-repeat: no-repeat;
        transition: all ease-in 0.25s;
        cursor: pointer;

        &:hover {
            background-color: #fff;
        }
    }

    .copy-password {
        bottom: 0;
        background-color: #fff;
        background-image: url('/copy-full.svg');
        background-size: 50%;
        background-position: center center;
        background-repeat: no-repeat;
        transition: all ease-in 0.25s;
        cursor: pointer;

        &:hover {
            background-color: #fff;
        }
    }

    .tooltip {
        font-size: 0.8em;
        display: block;
        text-align: center;
        padding: 0.5em;
        border-radius: 3px;
        position: absolute;
        bottom: -35px;
        left: 50%;
        transform: translateX(-50%);
    }

    .seperator {
        width: 100%;
        height: 3px;
        background-color: #fff;
        margin: 30px 0 15px;
    }

    footer {
        width: 100%;
        text-align: center;
        color: #fff;
    }

    footer a {
        color: #fff;
    }

    .github-links {
        margin: 30px 0 40px;
    }
</style>