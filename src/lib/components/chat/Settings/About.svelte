<script lang="ts">
	import { getVersionUpdates } from '$lib/apis';
	import { getOllamaVersion } from '$lib/apis/ollama';
	import { WEBUI_BUILD_HASH, WEBUI_VERSION } from '$lib/constants';
	import { WEBUI_NAME, config, showChangelog } from '$lib/stores';
	import { compareVersion } from '$lib/utils';
	import { onMount, getContext } from 'svelte';

	import Tooltip from '$lib/components/common/Tooltip.svelte';

	const i18n = getContext('i18n');

	let ollamaVersion = '';

	let updateAvailable = null;
	let version = {
		current: '',
		latest: ''
	};

	const checkForVersionUpdates = async () => {
		updateAvailable = null;
		version = await getVersionUpdates(localStorage.token).catch((error) => {
			return {
				current: WEBUI_VERSION,
				latest: WEBUI_VERSION
			};
		});

		console.log(version);

		updateAvailable = compareVersion(version.latest, version.current);
		console.log(updateAvailable);
	};

	onMount(async () => {
		ollamaVersion = await getOllamaVersion(localStorage.token).catch((error) => {
			return '';
		});

		checkForVersionUpdates();
	});
</script>

<div class="flex flex-col h-full justify-between space-y-3 text-sm mb-6">
	<div class=" space-y-3 overflow-y-scroll max-h-[28rem] lg:max-h-full">
		<div>
			<div class=" mb-2.5 text-sm font-medium flex space-x-2 items-center">
				<div>
					{$WEBUI_NAME}
					{$i18n.t('Version')}
				</div>
			</div>
			<div class="flex w-full justify-between items-center">
				<div class="flex flex-col text-xs text-gray-700 dark:text-gray-200">
					<div class="flex gap-1">
						<Tooltip content={WEBUI_BUILD_HASH}>
							v{WEBUI_VERSION}
						</Tooltip>

					
					</div>

				
				</div>

		
			</div>
		</div>

		{#if ollamaVersion}
			<hr class=" border-gray-100 dark:border-gray-850" />

			<div>
				<div class=" mb-2.5 text-sm font-medium">{$i18n.t('Ollama Version')}</div>
				<div class="flex w-full">
					<div class="flex-1 text-xs text-gray-700 dark:text-gray-200">
						{ollamaVersion ?? 'N/A'}
					</div>
				</div>
			</div>
		{/if}

		<hr class=" border-gray-100 dark:border-gray-850" />




		<div class="flex space-x-1">
			<a href="https://discord.gg/5rJgQTnV4s" target="_blank">
				<img
					alt="Telegram"
					src="https://img.shields.io/badge/Discord-Open_WebUI-blue?logo=discord&logoColor=white"
				/>
			</a>

			<a href="https://twitter.com/OpenWebUI" target="_blank">
				<img
					alt="X (formerly Twitter) Follow"
					src="https://img.shields.io/twitter/follow/OpenWebUI"
				/>
			</a>
		</div>


		<div>
			<pre
				class="text-xs text-gray-400 dark:text-gray-500">Copyright (c) {new Date().getFullYear()} <a
					href="https://ai-da.chat"
					target="_blank"
					class="underline">AiDa Chat</a
				>
Все права защищены.

ДАННОЕ ПРОГРАММНОЕ ОБЕСПЕЧЕНИЕ ПРЕДОСТАВЛЯЕТСЯ «КАК ЕСТЬ», 
БЕЗ КАКИХ-ЛИБО ГАРАНТИЙ, ЯВНЫХ ИЛИ ПОДРАЗУМЕВАЕМЫХ, ВКЛЮЧАЯ, НО НЕ ОГРАНИЧИВАЯСЬ, 
ГАРАНТИИ ТОВАРНОЙ ПРИГОДНОСТИ И ПРИГОДНОСТИ ДЛЯ ОПРЕДЕЛЕННОЙ ЦЕЛИ. 
НИ В КОЕМ СЛУЧАЕ ВЛАДЕЛЕЦ АВТОРСКИХ ПРАВ ИЛИ УЧАСТНИКИ НЕ НЕСУТ ОТВЕТСТВЕННОСТИ 
ЗА ЛЮБЫЕ ПРЯМЫЕ, КОСВЕННЫЕ, СЛУЧАЙНЫЕ, ОСОБЫЕ, ПРИМЕРНЫЕ ИЛИ КОСВЕННЫЕ УБЫТКИ 
(ВКЛЮЧАЯ, НО НЕ ОГРАНИЧИВАЯСЬ, ПРИОБРЕТЕНИЕ ЗАМЕНЯЮЩИХ ТОВАРОВ ИЛИ УСЛУГ; 
ПОТЕРЮ ИСПОЛЬЗОВАНИЯ, ДАННЫХ ИЛИ ПРИБЫЛИ; ИЛИ ПРЕРЫВАНИЕ БИЗНЕСА), 
КАКИМ-ЛИБО ОБРАЗОМ ВЫЗВАННЫЕ И НА ОСНОВАНИИ ЛЮБОЙ ТЕОРИИ ОТВЕТСТВЕННОСТИ, 
БУДЬ ТО В ДОГОВОРЕ, СТРОГОЙ ОТВЕТСТВЕННОСТИ ИЛИ ДЕЛИКТЕ (ВКЛЮЧАЯ ХАЛАТНОСТЬ ИЛИ ИНОЕ), 
ВОЗНИКАЮЩИЕ ЛЮБЫМ ОБРАЗОМ В РЕЗУЛЬТАТЕ ИСПОЛЬЗОВАНИЯ ДАННОГО ПРОГРАММНОГО ОБЕСПЕЧЕНИЯ, 
ДАЖЕ ЕСЛИ БЫЛО ПРЕДУПРЕЖДЕНИЕ О ВОЗМОЖНОСТИ ТАКОГО УЩЕРБА.

ПРИМЕЧАНИЕ: Все рекомендации, предоставляемые искусственным интеллектом (ИИ, АйдаЧатом, AidaChat), 
носят исключительно рекомендательный характер и требуют обязательной перепроверки и подтверждения 
со стороны квалифицированных специалистов. Разработчики и владельцы программного обеспечения 
не несут ответственности за последствия, вызванные использованием рекомендаций, предоставленных ИИ.
</pre>
		</div>

		<div class="mt-2 text-xs text-gray-400 dark:text-gray-500">
			{$i18n.t('Created by')}
			<a
				class=" text-gray-500 dark:text-gray-300 font-medium"
				href="https://ai-da.chat"
				target="_blank">AiDa chat</a
			>
		</div>
	</div>
</div>
