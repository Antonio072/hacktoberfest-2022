<script>
import { onMount } from 'svelte'

const DEFAULT_LENGTH = 8
const AVAILABLE_CHARACTERS = {
	lowercase: 'abcdefghijklmnopqrstuvwxyz',
	uppercase: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
	numbers: '0123456789',
	symbols: '!@#$%*+'
}

const DEFAULT_SETTINGS = {
	length: DEFAULT_LENGTH,
	lowercase: true,
	uppercase: true,
	numbers: true,
	symbols: false
}

const settings = DEFAULT_SETTINGS

let password = ''
let copied = false

function generatePassword() {
	let string = ''
	for (const [key, value] of Object.entries(AVAILABLE_CHARACTERS)) {
		if (DEFAULT_SETTINGS[key]) {
			string += value
		}

		let newPassword = ''
		for (let i = 0; i < DEFAULT_SETTINGS.length; i++) {
			newPassword += string[Math.floor(Math.random() * string.length)]
		}

		password = newPassword
	}
	copied = false
}

function copyToClipboard() {
	navigator.clipboard.writeText(password)
	copied = true
}

onMount(() => {
	generatePassword()
})

</script>
<main class="min-h-screen px-8 lg:px-0 flex items-center justify-center">
	<div class="lg:w-1/3 sm:w-80 min-w-min flex flex-col items-center bg-gray-100 shadow p-3 rounded gap-3">
		<h1 class="text-2xl">Generador de contraseñas 🔑</h1>
		<div class="w-full flex flex-row flex-row-wrap items-center justify-center gap-2 p-3">
			<p class="grow border-4 border-blue-900 rounded font-light text-lg md:text-2xl p-2">
				{password}
			</p>
			
			<div class="flex  items-center border-2 border-gray-300 rounded cursor-pointer drop-shadow-lg p-3 {copied ? "bg-green-400 text-white" : ""}" on:click={()=> copyToClipboard()}>
				<svg
					width="24"
					height="24"
					viewBox="0 0 24 24"
					xmlns="http://www.w3.org/2000/svg"
					xmlns:xlink="http://www.w3.org/1999/xlink"
					><rect width="24" height="24" stroke="none" opacity="0" />
					<g transform="matrix(1 0 0 1 12 12)">
						<path
							style="stroke: none; stroke-width: 1; stroke-dasharray: none; stroke-linecap: butt; stroke-dashoffset: 0; stroke-linejoin: miter; stroke-miterlimit: 4; fill: {copied ? "rgb(255,255,255)" : "rgb(0,0,0)"}; fill-rule: nonzero; opacity: 1;"
							transform=" translate(-12, -12)"
							d="M 4 2 C 2.895 2 2 2.895 2 4 L 2 18 L 4 18 L 4 4 L 18 4 L 18 2 L 4 2 z M 8 6 C 6.895 6 6 6.895 6 8 L 6 20 C 6 21.105 6.895 22 8 22 L 20 22 C 21.105 22 22 21.105 22 20 L 22 8 C 22 6.895 21.105 6 20 6 L 8 6 z M 8 8 L 20 8 L 20 20 L 8 20 L 8 8 z"
							stroke-linecap="round"
						/>
					</g>
				</svg>
				<p>{copied ? "Copiada": "Copiar" }</p>
			</div>
		</div>
		<p class="">Selecciona el número de carácteres de la contraseña</p>
		<span>
			8
			<input class="cursor-pointer py-auto" type="range" id="length" min="8" max="20" step="1" bind:value={settings.length} />
			20
		</span>
		<p >Carácteres: <span id="length-value">{settings.length}</span></p>
		<ul>
			<li>
				<label>
					<input type="checkbox" placeholder="" bind:checked={settings.uppercase}/>
					Mayúsculas
				</label>
			</li>
			<li>
				<label>
					<input type="checkbox" placeholder="" bind:checked={settings.numbers}/>
					Números 0-9
				</label>
			</li>
			<li>
				<label>
					<input type="checkbox" placeholder="" bind:checked={settings.symbols}/>
					Símbolos !@#$%^&*()
				</label>
			</li>
		</ul>
		<button
			on:click={() => generatePassword()}
			class="w-full bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded text-2xl shadow-lg"
			id="generate">Generar contraseña
		</button>
	</div>
</main>