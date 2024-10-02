<script lang="ts">
	import { onMount } from 'svelte';
	import Board from './Board.svelte';
	import TextField from './TextField.svelte';

	const dat = [
		{
			c: 'ก',
			e: 'k'
		},
		{
			c: 'ข',
			e: "k'"
		},
		{
			c: 'ค',
			e: 'K'
		},
		{
			c: 'ง',
			e: 'N'
		},
		{
			c: 'จ',
			e: 'j'
		},
		{
			c: 'ฉ',
			e: "c'"
		},
		{
			c: 'ช',
			e: 'c'
		},
		{
			c: 'ฌ',
			e: 'c,'
		},
		{
			c: 'ด',
			e: 'd'
		},
		{
			c: 'ฏ',
			e: 'D,'
		},
		{
			c: 'ฎ',
			e: 'D'
		},
		{
			c: 'ฐ',
			e: 't;'
		},
		{
			c: 'ฒ',
			e: 't:'
		},
		{
			c: 'ต',
			e: "t'"
		},
		{
			c: 'ถ',
			e: 't,'
		},
		{
			c: 'ท',
			e: 't'
		},
		{
			c: 'ธ',
			e: 'T'
		},
		{
			c: 'น',
			e: 'n'
		},
		{
			c: 'บ',
			e: 'b'
		},
		{
			c: 'ป',
			e: 'p'
		},
		{
			c: 'พ',
			e: "p'"
		},
		{
			c: 'ภ',
			e: 'P'
		},
		{
			c: 'ม',
			e: 'm'
		},
		{
			c: 'ย',
			e: 'y'
		},
		{
			c: 'ญ',
			e: 'Y'
		},
		{
			c: 'ร',
			e: 'R'
		},
		{
			c: 'ล',
			e: 'l'
		},
		{
			c: 'ว',
			e: 'v'
		},
		{
			c: 'ส',
			e: 's'
		},
		{
			c: 'ห',
			e: 'H'
		},
		{
			c: 'ฬ',
			e: "l'"
		},
		{
			c: 'อ',
			e: 'o'
		},
		{
			c: 'ฮ',
			e: 'h'
		},
		{
			c: 'า',
			e: 'r'
		},
		{
			c: 'อำ',
			e: "r'"
		},
		{
			c: 'เ',
			e: "a'"
		},
		{
			c: 'แ',
			e: 'aa'
		},
		{
			c: 'ะ',
			e: 'a'
		},
		{
			c: 'อิ',
			e: 'e'
		},
		{
			c: 'อี',
			e: 'E'
		},
		{
			c: 'อื',
			e: "e'"
		},
		{
			c: 'อึ',
			e: 'e,'
		},
		{
			c: 'อั',
			e: 'A'
		},
		{
			c: 'อ่',
			e: '1'
		},
		{
			c: 'อ้',
			e: '2'
		},
		{
			c: 'อ๊',
			e: '7'
		},
		{
			c: 'อ๋',
			e: '+'
		},
		{
			c: 'อ็',
			e: '8'
		},
		{
			c: 'อ์',
			e: '6'
		},
		{
			c: 'อุ',
			e: 'u'
		},
		{
			c: 'อู',
			e: 'U'
		},
		{
			c: 'ใ',
			e: 'I'
		},
		{
			c: 'ไ',
			e: 'i'
		},
		{
			c: 'โ',
			e: 'O'
		}
	];

	const chars = dat.map((o) => o.c);
	const eng = dat.map((o) => o.e);
	let value = '';
	let thaiValue = '';

	function charToThai(chr: string) {
		if (chr === ' ') {
			return ' ';
		}
		const th = dat.find((o) => o.e === chr)?.c || '';
		if (th === 'อ') {
			return 'อ';
		}
		return th.replace('อ', '');
	}

	function engToThai(eng: string) {
		// find all indexs of "a" in eng
		const chars: string[] = [];
		let last: string = '';
		const merge = (chr: string) => {
			const lastEle = chars.pop();
			chars.push(lastEle + chr);
			last = '';
		};
		eng.split('').forEach((chr) => {
			if (!"',:;".includes(chr)) {
				if (last === 'a' && chr === 'a') {
					merge(chr);
					return;
				}
				chars.push(chr);
				last = chr;
				return;
			}
			merge(chr);
			return;
		});
		// console.log(chars);

		const thai = chars.map((e) => charToThai(e)).join('');
		return thai;
	}

	// auto focus textfield

	onMount(() => {
		document.querySelector('input')?.focus();
	});

	function updateValue(v: string) {
		// alert(v)
		value = v;
		const thai = engToThai(value);
		thaiValue = thai;
		navigator.clipboard.writeText(thai);
	}
</script>

<h2 class="text-center py-4 pt-8 t1 text-white">Karaoke to Thai Convertor</h2>

<Board on:update={(e) => updateValue(value + e.detail)} {chars} {eng} />
<!-- <Board chars={vowels} eng={eng2.split('').map((e) => e.toUpperCase())} isDim={true} /> -->

<div class="row cc mx-8">
	<div class="col is jc max-w-[40rem] wf py-2">
		<div class="col is jc wf">
			<div class="row ic jb wf">
				<p class="tg">Karaoke converted here...</p>
				<p class="tg">Auto copied to your clipboard ✅</p>
			</div>
			<div class="ts p-2 bg rounded border wf">
				<p>{'->'}{thaiValue}</p>
			</div>
		</div>
		<div class="my-2 wf">
			<TextField
				bind:value
				placeholder="type karaoke here..."
				on:update={(d) => {
					value = d.detail;
					updateValue(value);
				}}
			/>
		</div>

		<p class="tg">
			Let's try:<br />
			"ken dE yU dE" {'->'} "กิน ดี ยู ดี" <br />
			"kle'n" {'->'} "กลืน"<br />
			"t+oN" {'->'} "ท๋อง"<br />
			"mr'm1r' Ibmak'rm" {'->'} "มำม่ำ ใบมะขาม"<br />
		</p>
	</div>
</div>