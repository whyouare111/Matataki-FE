<template>
  <div class="card">
    <div class="fl jsb">
      <div class="from-to">
        <c-user-popover :user-id="Number(card.from_uid)">
          <router-link
            :to="{ name: 'user-id', params: { id: card.from_uid } }"
            class="username"
            :class="!card.from_username && 'logout'"
            target="_blank"
          >
            {{ from_nickname }}
          </router-link>
        </c-user-popover>
        <svg-icon
          icon-class="transfer"
          class="info-icon"
        />
        <c-user-popover :user-id="Number(card.to_uid)">
          <router-link
            :to="{ name: 'user-id', params: { id: card.to_uid } }"
            class="username"
            :class="!card.to_username && 'logout'"
            target="_blank"
          >
            {{ to_nickname }}
          </router-link>
        </c-user-popover>
        <txHash
          v-if="card.tx_hash"
          :hash="card.tx_hash"
        />
      </div>
      <span class="amount">{{ amount }}</span>
    </div>
    <div class="fl jsb">
      <span class="time">{{ time }}</span>
      <!-- <span class="type">{{ type }}</span> -->
      <span class="symbol">{{ card.symbol }}</span>
    </div>
  </div>
</template>

<script>
// import { isNDaysAgo } from '@/common/methods';
import { precision } from '@/utils/precisionConversion'
import txHash from '@/components/tx_hash_popover/index'

export default {
  name: 'AssetCard',
  components: {
    txHash
  },
  props: {
    card: {
      type: Object,
      required: true
    }
  },
  computed: {
    time() {
      return this.moment(this.card.create_time).format('MMMDo HH:mm')
    },
    amount() {
      const tokenamount = precision(this.card.amount, 'CNY', this.card.decimals)
      return this.$publishMethods.formatDecimal(tokenamount, 4)
    },
    color() {
      return '#d74e5a'
    },
    type() {
      const { type } = this.card
      const typeList = {
        mint: '增发',
        transfer: '转账',
        exchange_purchase: '交易 ',
        exchange_addliquidity: '添加流动性',
        exchange_removeliquidity: '删除流动性',
        reward_article: '打赏'
      }
      return typeList[type] || '其他'
    },
    from_nickname() {
      return this.card.from_nickname || this.card.from_username || this.$t('error.accountHasBeenLoggedOut')
    },
    to_nickname() {
      return this.card.to_nickname || this.card.to_username || this.$t('error.accountHasBeenLoggedOut')
    }
  },
  created() {},
  methods: {}
}
</script>

<style scoped lang="less">
.card {
  margin: 10px 0;
  box-sizing: border-box;
  background-color: #fff;
  padding: 10px 0;
  display: flex;
  flex-direction: column;
  position: relative;
  text-align: left;
  width: 100%;
  border-bottom: 1px solid #DBDBDB;
  & > div {
    margin: 4px 0;
  }
}
.card-info {
  flex: 1;
  flex-direction: column;
  margin-left: 10px;
}
.info-icon {
  color: #000;
  margin: 0 6px;
}
.username {
  font-size:20px;
  font-weight:400;
  color:rgba(0,0,0,1);
  line-height:28px;
  &.logout {
    color: #b2b2b2;
  }
}
.type {
  font-size: 12px;
  font-weight: 400;
  color: rgba(178, 178, 178, 1);
  line-height: 17px;
  margin: 2px 0;
}
.amount {
  font-size:20px;
  font-weight:500;
  color:rgba(0,0,0,1);
  line-height:28px;
}
.time {
  font-size:16px;
  font-weight:400;
  color:rgba(178,178,178,1);
  line-height:22px;
}
.symbol {
  font-size:16px;
  font-weight:400;
  color:rgba(178,178,178,1);
  line-height:22px;
}

@media screen and (max-width: 540px) {
  .card > div {
    margin: 2px 0;
  }
  .username,
  .time {
    font-size: 12px;
  }
  .amount,
  .symbol {
    font-size: 14px;
  }
}
</style>
