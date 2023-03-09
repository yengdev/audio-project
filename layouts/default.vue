<template>
    <div>
        <div class="flex p-3 items-center">
            <div class="flex-none">
                <div class="text-2xl font-bold">19:00</div>
                <div class="text-sm text-slate-500">Wednesday, 08 Mar</div>
            </div>
            <div class="mx-10 grow hidden md:inline">
                <input
                    type="text"
                    placeholder="Search podcast"
                    class="focus:outline-none focus:w-full rounded-md p-3"
                />
            </div>
        </div>
        <div class="p-3 px-3 md:px-10">
            <slot />
        </div>
    </div>
</template>