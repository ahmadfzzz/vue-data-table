<template>
    <div :class="tableWrapperClass">
        <table :class="tableClass">
            <thead>
                <tr>
                    <th
                        v-for="(column, i) in columns" :key="i"
                        :data-sorting="column.sortingMode"
                        :class="{sortable: column.sortable}" class="column"
                        @click="$emit('sortColumn', column)"
                    >

                        <div class="column-content">
                            <span v-html="column.title"></span>

                            <component
                                :index="column.sortingIndex"
                                :is="sortingIndexComponent"
                                v-if="column.sortingIndex > 0" />
                            <component
                                :is="sortingIconComponent"
                                v-if="column.sortable" />
                        </div>
                    </th>

                    <th v-for="action in actionColumns" :key="action.name" class="action-column">
                        <span v-html="action.title"></span>
                    </th>

                    <th v-if="actionMode === 'single'" class="action-column">
                        <span v-html="actionColumn.title"></span>
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr v-if="isEmpty">
                    <td :colspan="numberOfColumns" class="empty-table-cell">
                        {{ emptyTableText }}
                    </td>
                </tr>

                <tr v-for="(data, i) in dataDisplayed" :key="i">

                    <!-- display action components in multiple cells (if actionMode is multiple) -->
                    <td class="action-cell" v-for="action in actionColumns" :key="action.name" >
                        <component
                            :is="action.component"
                            v-bind="{ data }"
                            @actionTriggered="actionTriggered"/>
                    </td>

                    <!-- display a single cell with all action components (if actionMode is single)-->
                    <td class="action-cell" v-if="(actionMode === 'single')">
                        <component
                            :is="component"
                            v-bind="{ data }"
                            v-for="(component, i) in actionColumn.components" :key="i"
                            @actionTriggered="actionTriggered"/>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script src="./Table.js"></script>
<style src="./Table.scss" scoped lang="scss"></style>
