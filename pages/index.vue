<template>
  <div>
    <p>ISBN：{{ code }}</p>
    <p>タイトル：{{ name }}</p>
    <p>著者：{{ person }}</p>
    <p>出版社：{{ publisher }}</p>
    <img :src="img" :alt="name">
  </div>
</template>

<script>
import axios from 'axios'

export default {
  async asyncData ({ params }) {
    const url = 'https://api.openbd.jp/v1/get?isbn=9784757544499'
    const { data } = await axios.get(url)

    const bookCode = data[0].onix.RecordReference
    const bookName = data[0].onix.DescriptiveDetail.TitleDetail.TitleElement.TitleText.content
    // TODO: 二人いる場合があるので、その処理を
    const bookPerson = data[0].onix.DescriptiveDetail.Contributor[0].PersonName.content
    const bookPublisher = data[0].onix.PublishingDetail.Imprint.ImprintName
    // const bookPrice = data[0].onix.ProductSupply.Price
    const bookImg = data[0].onix.CollateralDetail.SupportingResource[0].ResourceVersion[0].ResourceLink
    return { code: bookCode, name: bookName, person: bookPerson, publisher: bookPublisher, img: bookImg }
  }
}
</script>

<style>

</style>
