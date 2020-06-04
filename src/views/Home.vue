<template lang="pug">
	div
		form.flex(class="flex-col md:flex-row")
			.px-2.flex-grow.mt-8
				div
					select.w-full.h-10.px-2.mb-4.rounded-sm.bg-gray-200.cursor-pointer.border-2.border-transparent.outline-none.select-none(v-model="langFrom" class="focus:bg-transparent focus:border-gray-500" @change="translate")
						option(v-for="lang in langsFrom" :key="lang.id" :value="lang.code") {{ lang.name }}
				div
					textarea.w-full.resize-y.bg-gray-200.outline-none.border-2.border-transparent.rounded-sm.p-2(v-model="textFrom" class="focus:bg-transparent focus:border-gray-500")
			.px-2.flex-grow.mt-8
				div
					select.w-full.h-10.px-2.mb-4.rounded-sm.bg-gray-200.cursor-pointer.border-2.border-transparent.outline-none.select-none(v-model="langTo" class="focus:bg-white focus:border-gray-500" @change="translate")
						option(v-for="lang in langsTo" :key="lang.id" :value="lang.code") {{ lang.name }}
				div
					textarea.w-full.resize-y.bg-gray-200.outline-none.border-2.border-transparent.rounded-sm.p-2.text-gray-600(v-model="textTo" disabled ref="textTo")
		.mt-8
			.table-row
				.table-cell.px-2
					button.px-2.h-10.bg-gray-200.rounded-sm.border-2.border-transparent.select-none.uppercase.mr-4(v-show="!loading" @click="translate" class="focus:bg-transparent focus:border-gray-500 focus:outline-none") Translate
					button.px-2.h-10.bg-gray-200.text-gray-600.cursor-wait.rounded-sm.border-2.border-transparent.select-none.uppercase.mr-4(v-show="loading") Translating...
					button.px-2.h-10.bg-gray-200.rounded-sm.border-2.border-transparent.select-none.uppercase.mr-4(@click="copyTranslation" class="focus:bg-transparent focus:border-gray-500 focus:outline-none") Copy
		base-notification(:opened="showTranslationCopiedNotification") Copied.
		base-notification(:opened="showErrorNotification") An error occured.
</template>

<script lang="ts">
	import Vue from "vue";
	import { ILang } from "@/interfaces";
	import translate from "@khalyomede/translate";
	import { Lang } from "@khalyomede/translate/lib/types";
	import { BaseNotification } from "@/components";

	const langFrom: Lang = "en";
	const langTo: Lang = "fr";

	const langs: ILang[] = [
		{
			id: 129,
			name: "Afar",
			code: "aa",
		},
		{
			id: 130,
			name: "Abkhazian",
			code: "ab",
		},
		{
			id: 131,
			name: "Avestan",
			code: "ae",
		},
		{
			id: 132,
			name: "Afrikaans",
			code: "af",
		},
		{
			id: 133,
			name: "Akan",
			code: "ak",
		},
		{
			id: 134,
			name: "Amharic",
			code: "am",
		},
		{
			id: 135,
			name: "Aragonese",
			code: "an",
		},
		{
			id: 136,
			name: "Arabic",
			code: "ar",
		},
		{
			id: 137,
			name: "Assamese",
			code: "as",
		},
		{
			id: 138,
			name: "Avaric",
			code: "av",
		},
		{
			id: 139,
			name: "Aymara",
			code: "ay",
		},
		{
			id: 140,
			name: "Azerbaijani",
			code: "az",
		},
		{
			id: 141,
			name: "Bashkir",
			code: "ba",
		},
		{
			id: 142,
			name: "Belarusian",
			code: "be",
		},
		{
			id: 143,
			name: "Bulgarian",
			code: "bg",
		},
		{
			id: 144,
			name: "Bihari languages",
			code: "bh",
		},
		{
			id: 145,
			name: "Bislama",
			code: "bi",
		},
		{
			id: 146,
			name: "Bambara",
			code: "bm",
		},
		{
			id: 147,
			name: "Bengali",
			code: "bn",
		},
		{
			id: 148,
			name: "Tibetan",
			code: "bo",
		},
		{
			id: 149,
			name: "Breton",
			code: "br",
		},
		{
			id: 150,
			name: "Bosnian",
			code: "bs",
		},
		{
			id: 151,
			name: "Catalan; Valencian",
			code: "ca",
		},
		{
			id: 152,
			name: "Chechen",
			code: "ce",
		},
		{
			id: 153,
			name: "Chamorro",
			code: "ch",
		},
		{
			id: 154,
			name: "Corsican",
			code: "co",
		},
		{
			id: 155,
			name: "Cree",
			code: "cr",
		},
		{
			id: 156,
			name: "Czech",
			code: "cs",
		},
		{
			id: 157,
			name:
				"Church Slavic; Old Slavonic; Church Slavonic; Old Bulgarian; Old Church Slavonic",
			code: "cu",
		},
		{
			id: 158,
			name: "Chuvash",
			code: "cv",
		},
		{
			id: 159,
			name: "Welsh",
			code: "cy",
		},
		{
			id: 160,
			name: "Danish",
			code: "da",
		},
		{
			id: 161,
			name: "German",
			code: "de",
		},
		{
			id: 162,
			name: "Divehi; Dhivehi; Maldivian",
			code: "dv",
		},
		{
			id: 163,
			name: "Dzongkha",
			code: "dz",
		},
		{
			id: 164,
			name: "Ewe",
			code: "ee",
		},
		{
			id: 165,
			name: "Greek, Modern (1453-)",
			code: "el",
		},
		{
			id: 166,
			name: "English",
			code: "en",
		},
		{
			id: 167,
			name: "Esperanto",
			code: "eo",
		},
		{
			id: 168,
			name: "Spanish; Castilian",
			code: "es",
		},
		{
			id: 169,
			name: "Estonian",
			code: "et",
		},
		{
			id: 170,
			name: "Basque",
			code: "eu",
		},
		{
			id: 171,
			name: "Persian",
			code: "fa",
		},
		{
			id: 172,
			name: "Fulah",
			code: "ff",
		},
		{
			id: 173,
			name: "Finnish",
			code: "fi",
		},
		{
			id: 174,
			name: "Fijian",
			code: "fj",
		},
		{
			id: 175,
			name: "Faroese",
			code: "fo",
		},
		{
			id: 176,
			name: "French",
			code: "fr",
		},
		{
			id: 177,
			name: "Western Frisian",
			code: "fy",
		},
		{
			id: 178,
			name: "Irish",
			code: "ga",
		},
		{
			id: 179,
			name: "Gaelic; Scottish Gaelic",
			code: "gd",
		},
		{
			id: 180,
			name: "Galician",
			code: "gl",
		},
		{
			id: 181,
			name: "Guarani",
			code: "gn",
		},
		{
			id: 182,
			name: "Gujarati",
			code: "gu",
		},
		{
			id: 183,
			name: "Manx",
			code: "gv",
		},
		{
			id: 184,
			name: "Hausa",
			code: "ha",
		},
		{
			id: 185,
			name: "Hebrew",
			code: "he",
		},
		{
			id: 186,
			name: "Hindi",
			code: "hi",
		},
		{
			id: 187,
			name: "Hiri Motu",
			code: "ho",
		},
		{
			id: 188,
			name: "Croatian",
			code: "hr",
		},
		{
			id: 189,
			name: "Haitian; Haitian Creole",
			code: "ht",
		},
		{
			id: 190,
			name: "Hungarian",
			code: "hu",
		},
		{
			id: 191,
			name: "Armenian",
			code: "hy",
		},
		{
			id: 192,
			name: "Herero",
			code: "hz",
		},
		{
			id: 193,
			name: "Interlingua (International Auxiliary Language Association)",
			code: "ia",
		},
		{
			id: 194,
			name: "Indonesian",
			code: "id",
		},
		{
			id: 195,
			name: "Interlingue; Occidental",
			code: "ie",
		},
		{
			id: 196,
			name: "Igbo",
			code: "ig",
		},
		{
			id: 197,
			name: "Sichuan Yi; Nuosu",
			code: "ii",
		},
		{
			id: 198,
			name: "Inupiaq",
			code: "ik",
		},
		{
			id: 199,
			name: "Ido",
			code: "io",
		},
		{
			id: 1100,
			name: "Icelandic",
			code: "is",
		},
		{
			id: 1101,
			name: "Italian",
			code: "it",
		},
		{
			id: 1102,
			name: "Inuktitut",
			code: "iu",
		},
		{
			id: 1103,
			name: "Japanese",
			code: "ja",
		},
		{
			id: 1104,
			name: "Javanese",
			code: "jv",
		},
		{
			id: 1105,
			name: "Georgian",
			code: "ka",
		},
		{
			id: 1106,
			name: "Kongo",
			code: "kg",
		},
		{
			id: 1107,
			name: "Kikuyu; Gikuyu",
			code: "ki",
		},
		{
			id: 1108,
			name: "Kuanyama; Kwanyama",
			code: "kj",
		},
		{
			id: 1109,
			name: "Kazakh",
			code: "kk",
		},
		{
			id: 1110,
			name: "Kalaallisut; Greenlandic",
			code: "kl",
		},
		{
			id: 1111,
			name: "Central Khmer",
			code: "km",
		},
		{
			id: 1112,
			name: "Kannada",
			code: "kn",
		},
		{
			id: 1113,
			name: "Korean",
			code: "ko",
		},
		{
			id: 1114,
			name: "Kanuri",
			code: "kr",
		},
		{
			id: 1115,
			name: "Kashmiri",
			code: "ks",
		},
		{
			id: 1116,
			name: "Kurdish",
			code: "ku",
		},
		{
			id: 1117,
			name: "Komi",
			code: "kv",
		},
		{
			id: 1118,
			name: "Cornish",
			code: "kw",
		},
		{
			id: 1119,
			name: "Kirghiz; Kyrgyz",
			code: "ky",
		},
		{
			id: 1120,
			name: "Latin",
			code: "la",
		},
		{
			id: 1121,
			name: "Luxembourgish; Letzeburgesch",
			code: "lb",
		},
		{
			id: 1122,
			name: "Ganda",
			code: "lg",
		},
		{
			id: 1123,
			name: "Limburgan; Limburger; Limburgish",
			code: "li",
		},
		{
			id: 1124,
			name: "Lingala",
			code: "ln",
		},
		{
			id: 1125,
			name: "Lao",
			code: "lo",
		},
		{
			id: 1126,
			name: "Lithuanian",
			code: "lt",
		},
		{
			id: 1127,
			name: "Luba-Katanga",
			code: "lu",
		},
		{
			id: 1128,
			name: "Latvian",
			code: "lv",
		},
		{
			id: 1129,
			name: "Malagasy",
			code: "mg",
		},
		{
			id: 1130,
			name: "Marshallese",
			code: "mh",
		},
		{
			id: 1131,
			name: "Maori",
			code: "mi",
		},
		{
			id: 1132,
			name: "Macedonian",
			code: "mk",
		},
		{
			id: 1133,
			name: "Malayalam",
			code: "ml",
		},
		{
			id: 1134,
			name: "Mongolian",
			code: "mn",
		},
		{
			id: 1135,
			name: "Marathi",
			code: "mr",
		},
		{
			id: 1136,
			name: "Malay",
			code: "ms",
		},
		{
			id: 1137,
			name: "Maltese",
			code: "mt",
		},
		{
			id: 1138,
			name: "Burmese",
			code: "my",
		},
		{
			id: 1139,
			name: "Nauru",
			code: "na",
		},
		{
			id: 1140,
			name: "Bokmål, Norwegian; Norwegian Bokmål",
			code: "nb",
		},
		{
			id: 1141,
			name: "Ndebele, North; North Ndebele",
			code: "nd",
		},
		{
			id: 1142,
			name: "Nepali",
			code: "ne",
		},
		{
			id: 1143,
			name: "Ndonga",
			code: "ng",
		},
		{
			id: 1144,
			name: "Dutch; Flemish",
			code: "nl",
		},
		{
			id: 1145,
			name: "Norwegian Nynorsk; Nynorsk, Norwegian",
			code: "nn",
		},
		{
			id: 1146,
			name: "Norwegian",
			code: "no",
		},
		{
			id: 1147,
			name: "Ndebele, South; South Ndebele",
			code: "nr",
		},
		{
			id: 1148,
			name: "Navajo; Navaho",
			code: "nv",
		},
		{
			id: 1149,
			name: "Chichewa; Chewa; Nyanja",
			code: "ny",
		},
		{
			id: 1150,
			name: "Occitan (post 1500)",
			code: "oc",
		},
		{
			id: 1151,
			name: "Ojibwa",
			code: "oj",
		},
		{
			id: 1152,
			name: "Oromo",
			code: "om",
		},
		{
			id: 1153,
			name: "Oriya",
			code: "or",
		},
		{
			id: 1154,
			name: "Ossetian; Ossetic",
			code: "os",
		},
		{
			id: 1155,
			name: "Panjabi; Punjabi",
			code: "pa",
		},
		{
			id: 1156,
			name: "Pali",
			code: "pi",
		},
		{
			id: 1157,
			name: "Polish",
			code: "pl",
		},
		{
			id: 1158,
			name: "Pushto; Pashto",
			code: "ps",
		},
		{
			id: 1159,
			name: "Portuguese",
			code: "pt",
		},
		{
			id: 1160,
			name: "Quechua",
			code: "qu",
		},
		{
			id: 1161,
			name: "Romansh",
			code: "rm",
		},
		{
			id: 1162,
			name: "Rundi",
			code: "rn",
		},
		{
			id: 1163,
			name: "Romanian; Moldavian; Moldovan",
			code: "ro",
		},
		{
			id: 1164,
			name: "Russian",
			code: "ru",
		},
		{
			id: 1165,
			name: "Kinyarwanda",
			code: "rw",
		},
		{
			id: 1166,
			name: "Sanskrit",
			code: "sa",
		},
		{
			id: 1167,
			name: "Sardinian",
			code: "sc",
		},
		{
			id: 1168,
			name: "Sindhi",
			code: "sd",
		},
		{
			id: 1169,
			name: "Northern Sami",
			code: "se",
		},
		{
			id: 1170,
			name: "Sango",
			code: "sg",
		},
		{
			id: 1171,
			name: "Sinhala; Sinhalese",
			code: "si",
		},
		{
			id: 1172,
			name: "Slovak",
			code: "sk",
		},
		{
			id: 1173,
			name: "Slovenian",
			code: "sl",
		},
		{
			id: 1174,
			name: "Samoan",
			code: "sm",
		},
		{
			id: 1175,
			name: "Shona",
			code: "sn",
		},
		{
			id: 1176,
			name: "Somali",
			code: "so",
		},
		{
			id: 1177,
			name: "Albanian",
			code: "sq",
		},
		{
			id: 1178,
			name: "Serbian",
			code: "sr",
		},
		{
			id: 1179,
			name: "Swati",
			code: "ss",
		},
		{
			id: 1180,
			name: "Sotho, Southern",
			code: "st",
		},
		{
			id: 1181,
			name: "Sundanese",
			code: "su",
		},
		{
			id: 1182,
			name: "Swedish",
			code: "sv",
		},
		{
			id: 1183,
			name: "Swahili",
			code: "sw",
		},
		{
			id: 10,
			name: "Tamil",
			code: "ta",
		},
		{
			id: 11,
			name: "Telugu",
			code: "te",
		},
		{
			id: 12,
			name: "Tajik",
			code: "tg",
		},
		{
			id: 13,
			name: "Thai",
			code: "th",
		},
		{
			id: 14,
			name: "Tigrinya",
			code: "ti",
		},
		{
			id: 15,
			name: "Turkmen",
			code: "tk",
		},
		{
			id: 16,
			name: "Tagalog",
			code: "tl",
		},
		{
			id: 17,
			name: "Tswana",
			code: "tn",
		},
		{
			id: 18,
			name: "Tonga (Tonga Islands)",
			code: "to",
		},
		{
			id: 19,
			name: "Turkish",
			code: "tr",
		},
		{
			id: 110,
			name: "Tsonga",
			code: "ts",
		},
		{
			id: 111,
			name: "Tatar",
			code: "tt",
		},
		{
			id: 112,
			name: "Twi",
			code: "tw",
		},
		{
			id: 113,
			name: "Tahitian",
			code: "ty",
		},
		{
			id: 114,
			name: "Uighur; Uyghur",
			code: "ug",
		},
		{
			id: 115,
			name: "Ukrainian",
			code: "uk",
		},
		{
			id: 116,
			name: "Urdu",
			code: "ur",
		},
		{
			id: 117,
			name: "Uzbek",
			code: "uz",
		},
		{
			id: 118,
			name: "Venda",
			code: "ve",
		},
		{
			id: 119,
			name: "Vietnamese",
			code: "vi",
		},
		{
			id: 120,
			name: "Volapük",
			code: "vo",
		},
		{
			id: 121,
			name: "Walloon",
			code: "wa",
		},
		{
			id: 122,
			name: "Wolof",
			code: "wo",
		},
		{
			id: 123,
			name: "Xhosa",
			code: "xh",
		},
		{
			id: 124,
			name: "Yiddish",
			code: "yi",
		},
		{
			id: 125,
			name: "Yoruba",
			code: "yo",
		},
		{
			id: 126,
			name: "Zhuang; Chuang",
			code: "za",
		},
		{
			id: 127,
			name: "Chinese",
			code: "zh",
		},
		{
			id: 128,
			name: "Zulu",
			code: "zu",
		},
	];

	export default Vue.extend({
		components: {
			BaseNotification,
		},
		data() {
			return {
				langs: langs.sort(),
				langFrom,
				langTo,
				textFrom: "Hello",
				textTo: "Bonjour",
				loading: false,
				showTranslationCopiedNotification: false,
				showErrorNotification: false,
				translationCopiedNotificationTimeout: 0,
				errorNotificationTimeout: 0,
			};
		},
		created() {
			const savedTextFrom = localStorage.getItem("text-from");
			const savedTextTo = localStorage.getItem("text-to");
			const savedLangFrom = localStorage.getItem("lang-from");
			const savedLangTo = localStorage.getItem("lang-to");

			if (savedTextFrom) {
				try {
					this.textFrom = JSON.parse(savedTextFrom);
				} catch (exception) {
					this.textFrom = this.textFrom;
				}
			}

			if (savedTextTo) {
				try {
					this.textTo = JSON.parse(savedTextTo);
				} catch (exception) {
					this.textTo = this.textTo;
				}
			}

			if (savedLangFrom) {
				try {
					this.langFrom = JSON.parse(savedLangFrom);
				} catch (exception) {
					this.langFrom = this.langFrom;
				}
			}

			if (savedLangTo) {
				try {
					this.langTo = JSON.parse(savedLangTo);
				} catch (exception) {
					this.langTo = this.langTo;
				}
			}
		},
		computed: {
			langsFrom(): ILang[] {
				return this.langs;
			},
			langsTo(): ILang[] {
				return this.langs;
			},
		},
		watch: {
			textFrom(newValue) {
				localStorage.setItem("text-from", JSON.stringify(newValue));
			},
			textTo(newValue) {
				localStorage.setItem("text-to", JSON.stringify(newValue));
			},
			langFrom(newValue) {
				localStorage.setItem("lang-from", JSON.stringify(newValue));
			},
			langTo(newValue) {
				localStorage.setItem("lang-to", JSON.stringify(newValue));
			},
		},
		methods: {
			async translate() {
				this.loading = true;

				try {
					this.textTo = await translate(this.textFrom, {
						from: this.langFrom,
						to: this.langTo,
					});
				} catch (exception) {
					clearTimeout(this.errorNotificationTimeout);

					this.showErrorNotification = true;

					this.errorNotificationTimeout = setTimeout(
						() => (this.showErrorNotification = false),
						4000
					);
					/**
					 * @todo Send it to Sentry
					 */
				} finally {
					this.loading = false;
				}
			},
			copyTranslation() {
				this.$refs.textTo.removeAttribute("disabled");
				this.$refs.textTo.select();
				document.execCommand("copy");
				const selection = window.getSelection
					? window.getSelection()
					: document.selection
					? document.selection
					: null;
				if (!!selection)
					selection.empty
						? selection.empty()
						: selection.removeAllRanges();
				this.$refs.textTo.setAttribute("disabled", true);

				clearTimeout(this.translationCopiedNotificationTimeout);

				this.showTranslationCopiedNotification = true;

				this.translationCopiedNotificationTimeout = setTimeout(
					() => (this.showTranslationCopiedNotification = false),
					4000
				);
			},
		},
	});
</script>

<style lang="sass">
	@import "tailwindcss/base"
	@import "tailwindcss/components"
	@import "tailwindcss/utilities"
</style>
