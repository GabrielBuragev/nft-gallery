<template>
  <div class="unlockable-container">
    <Loader v-model="isLoading" :minted="justMinted" />
    <CountdownTimer />
    <hr class="text-color my-0" />
    <div class="container is-fluid">
      <div class="columns is-desktop">
        <div class="column is-half-desktop mobile-padding">
          <UnlockableCollectionInfo :collection-id="collectionId" />
          <hr class="mb-4" />

          <div
            class="is-flex is-justify-content-space-between is-align-items-center my-5">
            <span class="">Total available items</span>
            <span class=""
              >{{ totalAvailableMintCount }} / {{ totalCount }}</span
            >
          </div>
          <UnlockableTag />

          <div>
            <div
              class="is-flex is-justify-content-space-between is-align-items-center my-5">
              <span class="has-text-weight-bold is-size-5">First Phase</span
              ><span
                v-if="mintCountAvailable"
                class="is-flex is-align-items-center">
                <img src="/unlockable-pulse.svg" />
                Open</span
              >
            </div>
            <div
              class="is-flex is-justify-content-space-between is-align-items-center">
              <span>Free</span
              ><span class="has-text-weight-bold">
                {{ currentMintedCount }} / {{ MAX_PER_WINDOW }} Minted</span
              >
            </div>
          </div>
          <div class="my-5">
            <UnlockableSlider :value="currentMintedCount / MAX_PER_WINDOW" />
          </div>
          <div class="my-5">
            <template v-if="!hasUserMinted">
              <NeoButton
                ref="root"
                class="mb-2 mt-4 mint-button"
                variant="k-accent"
                :disabled="mintButtonDisabled"
                label="Mint"
                @click.native="handleSubmitMint" />
              <div class="is-flex is-align-items-center mt-2">
                <svg
                  width="20"
                  height="21"
                  class="mr-2"
                  viewBox="0 0 20 21"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg">
                  <path
                    d="M10 2.73016C14.4062 2.73016 18 6.32391 18 10.7302C18 15.1677 14.4062 18.7302 10 18.7302C5.5625 18.7302 2 15.1677 2 10.7302C2 9.07391 2.5 7.54266 3.375 6.26141L3.78125 5.63641L5.03125 6.48016L4.59375 7.10516C3.90625 8.13641 3.5 9.38641 3.5 10.7302C3.5 14.3239 6.40625 17.2302 10 17.2302C13.5625 17.2302 16.5 14.3239 16.5 10.7302C16.5 7.41766 13.9688 4.66766 10.75 4.29266V5.98016V6.73016H9.25V5.98016V3.48016V2.73016H10ZM8.03125 7.69891H8L10.5 10.1989L11.0312 10.7302L10 11.7927L9.46875 11.2614L6.96875 8.76141L6.4375 8.23016L7.5 7.16766L8.03125 7.69891Z"
                    fill="currentColor" />
                </svg>
                {{ leftTime }}
              </div>
            </template>
            <nuxt-link v-else :to="`/${urlPrefix}/gallery/${hasUserMinted}`">
              <p class="title is-size-4">
                [{{ $t('mint.unlockable.alreadyMinted') }}]
              </p>
            </nuxt-link>
          </div>

          <div class="my-5">
            <span class="has-text-weight-bold is-size-5">Schedule</span>
          </div>
          <div>
            <span> {{ $t('mint.unlockable.phaseIntroduction') }}</span>
          </div>
          <UnlockableSchedule />
        </div>
        <div class="column pt-5 is-flex is-justify-content-center">
          <ImageSlider
            v-if="imageList.length"
            :image-list="imageList"
            @select="handleSelectImage" />
        </div>
      </div>
      <hr class="text-color my-4" />
      <div class="columns is-desktop">
        <div
          class="column is-half-desktop is-flex is-flex-direction-column is-justify-content-center order-1">
          <div
            class="is-flex is-align-items-center has-text-weight-bold is-size-6 mb-2">
            <svg
              class="mr-2"
              width="20"
              height="21"
              viewBox="0 0 20 21"
              fill="none"
              xmlns="http://www.w3.org/2000/svg">
              <path
                d="M7.5 6.65137V8.65137H15.5H17V10.1514V17.1514V18.6514H15.5H4.5H3V17.1514V10.1514V8.65137H4.5H6V6.65137C6 4.46387 7.78125 2.65137 10 2.65137C11.7812 2.65137 13.2812 3.83887 13.7812 5.43262L12.375 5.90137C12.0312 4.90137 11.0938 4.15137 10 4.15137C8.59375 4.15137 7.5 5.27637 7.5 6.65137ZM4.5 17.1514H15.5V10.1514H4.5V17.1514Z"
                fill="currentColor" />
            </svg>
            How unlockable item works
          </div>
          <div>
            Experience the excitement of unlocking hidden rewards! Get your
            hands on exclusive merchandise (and an NFT!) linked to unlockable
            content. For the next ten hours, the fastest ten individuals can
            mint their very own anime waifu character NFT for free. Simply log
            in with your wallet, click on the "Mint" button, and sign the
            transaction. Afterward, check your profile to find the NFT and click
            "Unlockable Content" to reveal the surprise. Follow the schedule so
            you don't miss this!
          </div>
          <NeoButton
            tag="a"
            href="https://hello.kodadot.xyz/fandom-toolbox/audience-growth/unlockables"
            target="_blank"
            variant="secondary"
            class="mt-2">
            Learn More
          </NeoButton>
        </div>
        <div class="column">
          <img src="/unlockable-introduce.svg" alt="Unlockable" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import UnlockableCollectionInfo from '@/components/collection/unlockable/UnlockableCollectionInfo.vue'
import UnlockableTag from '@/components/collection/unlockable/UnlockableTag.vue'
import CountdownTimer from '@/components/collection/unlockable/CountdownTimer.vue'
import ImageSlider from '@/components/collection/unlockable/ImageSlider.vue'
import UnlockableSlider from '@/components/collection/unlockable/UnlockableSlider.vue'
import UnlockableSchedule from '@/components/collection/unlockable/UnlockableSchedule.vue'
import { doWaifu, getLatestWaifuImages } from '@/services/waifu'
import { DISPLAY_SLIDE_IMAGE_COUNT, collectionId, countDownTime } from './const'
import {
  UNLOCKABLE_CAMPAIGN,
  createUnlockableMetadata,
  getRandomInt,
  unlockableDesc,
} from './utils'
import { endOfHour, startOfHour } from 'date-fns'
import type Vue from 'vue'
import { ConnectWalletModalConfig } from '@/components/common/ConnectWallet/useConnectWallet'
import { NeoButton } from '@kodadot1/brick'
import { useCountDown } from './utils/useCountDown'

const Loader = defineAsyncComponent(
  () => import('@/components/collection/unlockable/UnlockableLoader.vue')
)
const { $neoModal } = useNuxtApp()
const root = ref<Vue<Record<string, string>>>()

const { toast } = useToast()

const imageList = ref<string[]>([])
const resultList = ref<any[]>([])
const selectedImage = ref('')
const MAX_PER_WINDOW = 10
const { urlPrefix } = usePrefix()

const isLoading = ref(false)
const { accountId, isLogIn } = useAuth()
const { hours, minutes, seconds } = useCountDown(countDownTime)
const justMinted = ref('')

onMounted(async () => {
  const res = await getLatestWaifuImages()
  imageList.value = res.result
    .slice(0, DISPLAY_SLIDE_IMAGE_COUNT)
    .map((item) => item.output)
  resultList.value = res.result
})

const leftTime = computed(() => {
  const hoursLeft = hours.value > 0 ? `${hours.value} Hour ` : ''
  const minutesLeft = minutes.value > 0 ? `${minutes.value} Minute ` : ''
  const secondsLeft = seconds.value > 0 ? `${seconds.value} Sec ` : ''
  const isFinish = !hoursLeft && !minutesLeft && !secondsLeft
  return isFinish ? 'Finished' : `${hoursLeft}${minutesLeft}${secondsLeft}Left`
})

const now = new Date()
const windowRange = [startOfHour(now), endOfHour(now)]

const handleSelectImage = (image: string) => {
  selectedImage.value = image
}

const { data: collectionData } = useGraphql({
  queryName: 'unlockableCollectionById',
  variables: {
    id: collectionId,
  },
})

const { data: stats, refetch: tryAgain } = useGraphql({
  queryName: 'firstNftOwnedByAccountAndCollectionId',
  variables: {
    id: collectionId,
    account: accountId.value,
  },
})

const hasUserMinted = computed(
  () => stats.value?.collection.nfts?.at(0)?.id || justMinted.value
)

const totalCount = computed(
  () => collectionData.value?.collectionEntity?.max || 300
)
const totalAvailableMintCount = computed(
  () =>
    totalCount.value -
    (collectionData.value?.nftEntitiesConnection?.totalCount || 0)
)

const { data, refetch } = useGraphql({
  queryName: 'collectionMintedBetween',
  clientName: urlPrefix.value,
  variables: {
    id: collectionId,
    from: windowRange[0],
    to: windowRange[1],
  },
})

const refetchData = async () => {
  console.log('refetch')
  await refetch()
  await tryAgain()
}

useSubscriptionGraphql({
  query: `collectionEntityById(id: "${collectionId}") {
    nftCount
  }`,
  onChange: refetchData,
})

const mintedCount = computed(() => data.value?.minted?.count || 0)

const currentMintedCount = computed(() =>
  Math.min(mintedCount.value, MAX_PER_WINDOW)
)

const mintCountAvailable = computed(
  () => currentMintedCount.value < MAX_PER_WINDOW
)

const mintButtonDisabled = computed(() =>
  Boolean(!mintCountAvailable.value || hasUserMinted.value)
)

const scrollToTop = () => {
  window.scroll({
    top: 0,
    behavior: 'smooth',
  })
}

const description = unlockableDesc(40)

const handleSubmitMint = async () => {
  if (!isLogIn.value) {
    $neoModal.open({
      parent: root?.value,
      ...ConnectWalletModalConfig,
    })
    return
  }
  if (isLoading.value) {
    return false
  }
  isLoading.value = true

  const randomIndex = getRandomInt(imageList.value.length - 1)
  const image = resultList.value.at(randomIndex).image

  const hash = await createUnlockableMetadata(image, description)

  const { accountId } = useAuth()

  try {
    const id = await doWaifu(
      {
        address: accountId.value,
        metadata: hash,
        image: image,
      },
      UNLOCKABLE_CAMPAIGN
    ).then((res) => {
      toast('mint success', 'is-neo', 20000)
      scrollToTop()
      return `${collectionId}-${res.result.sn}`
    })
    // 40s timeout
    setTimeout(() => {
      isLoading.value = false
      justMinted.value = id
      toast('You will be redirected in few seconds', 'is-neo', 3000)
      return navigateTo(`/${urlPrefix.value}/gallery/${id}`)
    }, 44000)
  } catch (error) {
    toast('failed to mint', 'is-neo', 20000)
    isLoading.value = false
  }
}
</script>

<style scoped lang="scss">
@import '@/styles/abstracts/variables';

.unlockable-container {
  .mint-button {
    width: 14rem;
    height: 3.5rem;
  }
}

.order-1 {
  order: 1;
}
</style>
