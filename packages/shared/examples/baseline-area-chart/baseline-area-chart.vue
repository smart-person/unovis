<script setup lang="ts">
import { CurveType } from '@unovis/ts'
import { VisXYContainer, VisArea, VisAxis, VisBulletLegend } from '@unovis/vue'
import type { Category, DataRecord } from './data'
import { data, categories } from './data'

// Configure accessors and baseline
const sums = data.map(d => d.art.reduce((t, i) => t + i, 0))
const max = Math.max(...sums)

// Area
const x = d => d.year
const y = categories.map((c: Category) => d => d.art[c.id])
const baseline = (_: DataRecord, i: number) => (max - sums[i]) / 2

// Y Axis
const yAxisConfig = {
  tickValues: Array(Math.round(max / 1000)).fill(0).map((_, i) => {
    const dir = i % 2 === 1 ? -(i - 1) : i
    return max / 2 + dir * 1000
  }),
  tickFormat: (i: number) => `${Math.abs(i - max / 2)}`,
}
</script>

<template>
  <VisBulletLegend :items="categories" />
  <VisXYContainer :data="data" :height="500">
    <VisArea :x="x" :y="y" :baseline="baseline" :curveType="CurveType.Basis" />
    <VisAxis type='x' label='Year' />
    <VisAxis type='y' label='Number of Works Acquired' v-bind="yAxisConfig" />
  </VisXYContainer>
</template>
