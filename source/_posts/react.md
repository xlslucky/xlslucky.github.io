---
title: react
date: 2017-11-21 19:29:52
tags:
  - react
categories: "react"
password: 123
---

## React constructor(){}
当你在React class中需要设置state的初始值或者绑定事件时，需要加上constructor(){}
`super()` 目的： 可以使用 `this.state`
`super(props)` 目的：在 `constructor` 中能使用 `this.props`


// 递归文件树
```js
const TREEOBJ = {
  name: 'root',
  children: [
    {
      name: 'c-1',
      children: [
        {
          name: 'c-1-1',
          children: [
            {
              name: 'c-1-1-1',
            },
            {
              name: 'c-1-1-2',
            }
          ]
        },
        {
          name: 'c-1-2',
          children: [
            {
              name: 'c-1-2-1',
            },
            {
              name: 'c-1-2-2',
              children: [
                {
                  name: 'c-1-2-2-1',
                }
              ]
            }
          ]
        }
      ]
    },
    {
      name: 'c-2',
      children: [
        {
          name: 'c-2-1',
        },
        {
          name: 'c-2-2',
        }
      ]
    }
  ]
}
export default () => {
  return (
    <div>
      <ul>
        <RenderTree treeObj={TREEOBJ} />
      </ul>
    </div>
  )
}

const RenderTree = (props) => {
  let obj = props.treeObj
  return (
    <li>
      {obj.name}
      {
        obj.children ? 
        <ul>
        {
          obj.children.map((item, index) => {
            return <RenderTree key={index} treeObj={item} />
          })
        }
        </ul> : null
      }
    </li>
  )
}
```

// 文件树拖拽
```js
import React from 'react'
import { SortableContainer, SortableElement, SortableHandle, arrayMove } from 'react-sortable-hoc'

const tree = { "folderId": 1, "name": "个人文件", "read": true, "write": true, "template": false, "fullRoute": "/1/", "childs": [{ "folderId": 5, "name": "一级文件夹", "read": true, "write": true, "template": false, "fullRoute": "/1/5/", "childs": [{ "folderId": 6, "name": "一级的1级文件夹", "read": true, "write": true, "template": false, "fullRoute": "/1/5/6/", "childs": [] }, { "folderId": 62, "name": "一级的2级文件夹", "read": true, "write": true, "template": false, "fullRoute": "/1/5/6/", "childs": [] }] }, { "folderId": 7, "name": "二级文件夹", "read": true, "write": true, "template": false, "fullRoute": "/1/7/", "childs": [{ "folderId": 353, "name": "二级1111", "read": true, "write": true, "template": false, "fullRoute": "/1/7/353/", "childs": [{ "folderId": 374, "name": "个人文件", "read": true, "write": true, "template": false, "fullRoute": "/1/7/353/374/", "childs": [] }] }] }, { "folderId": 318, "name": "zhaopy", "read": true, "write": true, "template": false, "fullRoute": "/1/318/", "childs": [] }, { "folderId": 102, "name": "习题集1", "read": true, "write": true, "template": false, "fullRoute": "/1/102/", "childs": [] }, { "folderId": 103, "name": "思想政治", "read": true, "write": true, "template": false, "fullRoute": "/1/103/", "childs": [{ "folderId": 139, "name": "思想政治子文件夹", "read": true, "write": true, "template": false, "fullRoute": "/1/103/139/", "childs": [] }] }, { "folderId": 363, "name": "xuls", "read": true, "write": true, "template": false, "fullRoute": "/1/363/", "childs": [] }] }

class Item extends React.Component {
  onSortEnd = ({oldIndex, newIndex}) => {
    const { childs } = this.props.folder
    arrayMove(childs, oldIndex, newIndex)
  };

  render() {
    const { folder } = this.props
    return (
      <li>
        <div>{folder.name}</div>
        {
          folder.childs.length > 0 &&
          <TreeList folders={folder.childs} onSortEnd={this.onSortEnd}/>
        }
      </li>
    )
  }
}

const TreeItem = SortableElement(({folder}) => <Item folder={folder}/>);

const TreeList = SortableContainer(({folders}) => {
  return (
    <ul>
      {
        folders.map((item, index) => {
          return <TreeItem index={index} key={index} folder={item} />
        })
      }
    </ul>
  )
})

export default class MoveTree extends React.Component {

  render() {
    return (
      <ul>
        <Item folder={tree} />
      </ul>
    )
  }
}

```