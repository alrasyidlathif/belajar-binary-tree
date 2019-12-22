# belajar-binary-tree
`class TreeNode {`

`.. constructor(val, left=undefined, right=undefined) {`

`.... this.val = val`

`.... this.left = left`

`.... this.right = right`

`.. }`

`}`

// how to make a tree
// node1 = new TreeNode(1)
// node2 = new TreeNode(10)
// node0 = new TreeNode(5, node1, node2)
// node3 = new TreeNode(8)
// node2.left = node3

`function search(value, tree=undefined) {`

`..  if (tree == undefined) {`

`....    return console.log('not found')`

`..  } else if (tree.val == value) {`

`....    return tree`
  } else if (tree.val > value) {
    return search(value, tree.left)
  } else {
    return search(value, tree.right)
  }
}

// how to search
// search(3)
// not found
// search(6, node0)
// not found
// search(8, nodeUpper)
// TreeNode { val: 8, left: undefined, right: undefined }

function insert(value, node) {
  if (value < node.val) {
    if (node.left != undefined) {
      return insert(value, node.left)
    } else {
      console.log('success insert ' + value)
      return node.left = new TreeNode(value)
    }
  } else if (value > node.val) {
    if (node.right != undefined) {
      return insert(value, node.right)
    } else {
      console.log('success insert ' + value)
      return node.right = new TreeNode(value)
    }
  } else {
    return console.log('value already in tree')
  }
}

// how to insert
// insert(5)
// Thrown:
// TypeError: Cannot read property 'val' of undefined
//    at insert (repl:2:20)

// insert(5, node0)
// value already in tree
// insert(12, node0)
// success insert 12
// TreeNode { val: 12, left: undefined, right: undefined }
// node0
// TreeNode {
//   val: 5,
//   left: TreeNode { val: 1, left: undefined, right: undefined },
//   right: TreeNode {
//     val: 10,
//     left: TreeNode { val: 8, left: undefined, right: undefined },
//     right: TreeNode { val: 12, left: undefined, right: undefined }
//   }
// }

function deleteNode(value, node) {
  if (node == undefined) {
    return console.log('value not found')
  } else if (value < node.val) {
    node.left = deleteNode(value, node.left)
    return node
  } else if (value > node.val) {
    node.right = deleteNode(value, node.right)
    return node
  } else if (value == node.val) {
    if (node.left == undefined) {
      return node.right
    } else if (node.right == undefined) {
      return node.left
    } else {
      node.right = liftNode(node.right, node)
      return node
    }
  }
}

function liftNode(node, nodeToDelete) {
  if (node.left) {
    node.left = liftNode(node.left, nodeToDelete)
    return node
  } else {
    nodeToDelete.val = node.val
    return node.right
  }
}

// how to delete
// node0
// TreeNode {
//   val: 5,
//   left: TreeNode { val: 1, left: undefined, right: undefined },
//   right: TreeNode {
//     val: 10,
//     left: TreeNode { val: 8, left: undefined, right: undefined },
//     right: undefined
//   }
// }
// deleteNode(5, node0)
// TreeNode {
//   val: 8,
//   left: TreeNode { val: 1, left: undefined, right: undefined },
//   right: TreeNode { val: 10, left: undefined, right: undefined }
// }
// deleteNode(1, node0)
// TreeNode {
//   val: 8,
//   left: undefined,
//   right: TreeNode { val: 10, left: undefined, right: undefined }
// }
// deleteNode(7, node0)
// value not found
// TreeNode {
//   val: 8,
//   left: undefined,
//   right: TreeNode { val: 10, left: undefined, right: undefined }
// }

// delete function by me (not done yet)
function deleteNode(value, node) {
  if (node == undefined) {
    return console.log('value not found')
  } else if (value < node.val) {
    return deleteNode(value, node.left)
  } else if (value > node.val) {
    return deleteNode(value, node.right)
  } else if (value == node.val) {
    if (node.left == undefined && node.right == undefined) {
      node.val = undefined
      return console.log('success delete node')
    } else if (node.left == undefined) {
      node.val = node.right.val
      node.left = node.right.left
      node.right = node.right.right
      return console.log('success delete node')
    } else if (node.right == undefined) {
      node.val = node.left.val
      node.left = node.left.left
      node.right = node.left.right
      return console.log('success delete node')
    } else {
      res = liftNode(node.right)
      node.val = res.newVal
      node.right = res.newNode
      return console.log('success delete node')
    }
  }
}

function liftNode(node) {
  if (node.left) {
    res = liftNode(node.left)
    node.left = res.newNode
    res = {
      ...res,
      newNode: node
    }
    return res
  } else {
    res = {
      newVal: node.val,
      newNode: node.right
    }
    return res
  }
}

// node0
// TreeNode {
//   val: 5,
//   left: TreeNode { val: 1, left: undefined, right: undefined },
//   right: TreeNode {
//     val: 10,
//     left: TreeNode { val: 8, left: undefined, right: undefined },
//     right: undefined
//   }
// }
// deleteNode(5, node0)
// success delete node
// > node0
// TreeNode {
//   val: 8,
//   left: TreeNode { val: 1, left: undefined, right: undefined },
//   right: TreeNode { val: 10, left: undefined, right: undefined }
// }
// deleteNode(7, node0)
// value not found
// deleteNode(1, node0)
// success delete node
// node0
// TreeNode {
//   val: 8,
//   left: TreeNode { val: undefined, left: undefined, right: undefined },
//   right: TreeNode { val: 10, left: undefined, right: undefined }
// }

// delete function in python from book
def delete(valueToDelete, node):
  // The base case is when we've hit the bottom of the tree,
  // and the parent node has no children:
  if node is None:
    return None
  
  // If the value we're deleting is less or greater than the current node,
  // we set the left or right child respectively to be
  // the return value of a recursive call of this very method on the current
  // node's left or right subtree.
  elif valueToDelete < node.value:
    node.leftChild = delete(valueToDelete, node.leftChild)
    // We return the current node (and its subtree if existent) to
    // be used as the new value of its parent's left or right child:
    return node
  
  elif valueToDelete > node.value:
    node.rightChild = delete(valueToDelete, node.rightChild)
    return node

  // If the current node is the one we want to delete:
  elif valueToDelete == node.value:
    // If the current node has no left child, we delete it by
    // returning its right child (and its subtree if existent)
    // to be its parent's new subtree:
    if node.leftChild is None:
      return node.rightChild

    // (If the current node has no left OR right child, this ends up
    // being None as per the first line of code in this function.)
    elif node.rightChild is None:
      return node.leftChild

    // If the current node has two children, we delete the current node
    // by calling the lift function (below), which changes the current node's
    // value to the value of its successor node:
    else:
      node.rightChild = lift(node.rightChild, node)
      return node


def lift(node, nodeToDelete):
  // If the current node of this function has a left child,
  // we recursively call this function to continue down
  // the left subtree to find the successor node.
  if node.leftChild:
    node.leftChild = lift(node.leftChild, nodeToDelete)
    return node

  // If the current node has no left child, that means the current node
  // of this function is the successor node, and we take its value
  // and make it the new value of the node that we're deleting:
  else:
    nodeToDelete.value = node.value
    // We return the successor node's right child to be now used
    // as its parent's left child:
    return node.rightChild`
