<script lang="ts">
	import { toast } from 'svelte-sonner';
	import { updateUserPassword } from '$lib/apis/auths';
	import { _ } from "svelte-i18n";

	let show = false;
	let currentPassword = '';
	let newPassword = '';
	let newPasswordConfirm = '';

	const updatePasswordHandler = async () => {
		if (newPassword === newPasswordConfirm) {
			const res = await updateUserPassword(localStorage.token, currentPassword, newPassword).catch(
				(error) => {
					toast.error(error);
					return null;
				}
			);

			if (res) {
				toast.success($_("alert.updateSuccess"));
			}

			currentPassword = '';
			newPassword = '';
			newPasswordConfirm = '';
		} else {
			toast.error(
				$_("alert.passwordError")
			);
			newPassword = '';
			newPasswordConfirm = '';
		}
	};
</script>

<form
	class="flex flex-col text-sm"
	on:submit|preventDefault={() => {
		updatePasswordHandler();
	}}
>
	<div class="flex justify-between items-center text-sm">
		<div class="  font-medium">{$_("message.changePassword")}</div>
		<button
			class=" text-xs font-medium text-gray-500"
			type="button"
			on:click={() => {
				show = !show;
			}}>{show ? $_("btn.hide") : $_("btn.show")}</button
		>
	</div>

	{#if show}
		<div class=" py-2.5 space-y-1.5">
			<div class="flex flex-col w-full">
				<div class=" mb-1 text-xs text-gray-500">{$_("message.currentPassword")}</div>

				<div class="flex-1">
					<input
						class="w-full rounded py-2 px-4 text-sm dark:text-gray-300 dark:bg-gray-800 outline-none"
						type="password"
						bind:value={currentPassword}
						autocomplete="current-password"
						required
					/>
				</div>
			</div>

			<div class="flex flex-col w-full">
				<div class=" mb-1 text-xs text-gray-500">{$_("message.newPassword")}</div>

				<div class="flex-1">
					<input
						class="w-full rounded py-2 px-4 text-sm dark:text-gray-300 dark:bg-gray-800 outline-none"
						type="password"
						bind:value={newPassword}
						autocomplete="new-password"
						required
					/>
				</div>
			</div>

			<div class="flex flex-col w-full">
				<div class=" mb-1 text-xs text-gray-500">{$_("message.confirmPassword")}</div>

				<div class="flex-1">
					<input
						class="w-full rounded py-2 px-4 text-sm dark:text-gray-300 dark:bg-gray-800 outline-none"
						type="password"
						bind:value={newPasswordConfirm}
						autocomplete="off"
						required
					/>
				</div>
			</div>
		</div>

		<div class="mt-3 flex justify-end">
			<button
				class=" px-4 py-2 text-xs bg-gray-800 hover:bg-gray-900 dark:bg-gray-700 dark:hover:bg-gray-800 text-gray-100 transition rounded-md font-medium"
			>
				{$_("btn.updatePassword")}
			</button>
		</div>
	{/if}
</form>
