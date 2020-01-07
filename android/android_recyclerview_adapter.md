## Android RecyclerView Adapter

함께 일하게된 인턴분이 여쭤보셔서 찾아보게된 RecyclerView에서 Mutli ViewType을 사용할 때 else를 없앨 수 있는 깔끔한 방법에 대해 예제를 짜보았다.


```kotlin
package net.daum.android.daum.home

import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.recyclerview.widget.RecyclerView
import net.daum.android.daum.R


/**
 * Created by Latte on 2020-01-07.
 */

class CustomAdapter(
    private val typeAClick: (String) -> Unit
) : RecyclerView.Adapter<CustomViewHolder>() {
    val items = mutableListOf<CustomData>()

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): CustomViewHolder {
        val type = CustomViewType.values()[viewType]
        val v = LayoutInflater.from(parent.context).inflate(type.resId, parent, false)
        return when (type) {
            CustomViewType.TypeA -> {
                CustomViewHolder.ViewHolderA(v,typeAClick) 
            }

            CustomViewType.TypeB -> {
                CustomViewHolder.ViewHolderB(v)
            }

            CustomViewType.TypeC -> {
                CustomViewHolder.ViewHolderC(v) {
                    print("onClick")
                }
            }
        }
    }

    override fun getItemCount(): Int {
        return items.size
    }

    override fun onBindViewHolder(holder: CustomViewHolder, position: Int) {
        holder.onBind(items[position])

    }

    override fun getItemViewType(position: Int): Int {
        return items[position].customViewType.ordinal
    }


}


enum class CustomViewType(val resId: Int) {
    TypeA(R.layout.layout_emoticon_featured_res),
    TypeB(R.layout.layout_emoticon_featured_res),
    TypeC(R.layout.layout_emoticon_featured_res)
}

sealed class CustomData(val customViewType: CustomViewType) {
    data class DataA(val val1: Int) : CustomData(CustomViewType.TypeA)
    data class DataB(val val2: String, val val1: Int) : CustomData(CustomViewType.TypeB)
    data class DataC(val val3: String, val fun1: () -> Unit) : CustomData(CustomViewType.TypeC)
}

sealed class CustomViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
    open fun onBind(item: CustomData) {}

    class ViewHolderA(itemView: View, clickListener: (String) -> Unit) :
        CustomViewHolder(itemView) {

        override fun onBind(item: CustomData) {
            if (item is CustomData.DataA) {
                item.val1
            }
        }
    }

    class ViewHolderB(itemView: View) : CustomViewHolder(itemView) {

        override fun onBind(item: CustomData) {
            if (item is CustomData.DataB) {
                item.val1
                item.val2
            }
        }
    }

    class ViewHolderC(itemView: View, clickListener: (String) -> Unit) :
        CustomViewHolder(itemView) {
        var val1 = ""

        init {
            itemView.setOnClickListener {
                clickListener(val1)
            }
        }

        override fun onBind(item: CustomData) {
            if (item is CustomData.DataC) {
                val1 = item.val3
            }
        }
    }
}
```