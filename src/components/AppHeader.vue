<script setup lang="ts">

</script>

<template>
    <header>
        <div class="container">
            <div class="flex justify-content-between align-items-center">
                <div class="logo">
                    <h2>Calendar</h2>
                </div>

                <div class="user">
                    <div class="avatar">
                        <i class="fa-solid fa-user"></i>
                    </div>
                    <div class="name">
                        <h3>
                            Pinco Pallo
                        </h3>
                        <p class="text-light">Admin</p>
                    </div>
                </div>
            </div>
        </div>
    </header>
</template>

<style lang="scss" scoped>
@use "../styles/partials/variables" as *;

header {
    padding: .8rem 0;
    border-bottom: 1px solid lighten($text-light-color, 20%);
}

.logo h2 {
    font-weight: 300;
    font-size: 1rem;
}

.user {
    display: flex;
    align-items: center;

    .avatar {
        width: 40px;
        height: 40px;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: lighten($primary-color, 30%);
        border-radius: 10px;
    }

    .name {
        padding-left: .5rem;

        h3,
        p {
            font-size: .8rem;
            font-weight: 300;
        }
    }
}
</style>